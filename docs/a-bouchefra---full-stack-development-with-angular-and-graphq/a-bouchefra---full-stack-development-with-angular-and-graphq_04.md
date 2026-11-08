## 3 连接数据库与 TypeORM

In the last chapter, we learned how to set up a Node.js server with GraphQL support to implement a backend application. Additionally, we explained how to install Express.js and configure it with TypeScript and GraphQL. We used mocking to provide a working GraphQL server with Apollo Server before implementing the resolvers that should actually be responsible for fetching and adding data.

In addition, we set up Cross-Origin Resource Sharing (CORS) and used Apollo Studio to communicate with our GraphQL server.

In this chapter, we'll look at how to connect a MySQL database to our application using TypeORM, as well as how to create resolvers to get and save data from the database.

Now that we've built a backend application that can respond to GraphQL queries, we'll need to connect it to our database in order to store our data. We'll utilize TypeORM to abstract the database operations, allowing you to use any desired database management system for your app without changing the code. In addition, we will learn how to connect TypeORM with Apollo, as well as how to generate and seed our database tables.

We will discuss the following topics in this chapter:

- Creating a MySQL user and database
- Setting up TypeORM and MySQL in our server project
- Creating TypeORM entities
- Seeding test data in our database
- Using Apollo together with TypeORM

## 技术要求

Before you can proceed with the steps in this chapter, you must first finish the steps in the previous chapter on how to set up a functional GraphQL server. Please refer to Chapter 2, Setting Up GraphQL with Node.js, Express.js, and Apollo, to learn how to build up a GraphQL server using Express.js and Apollo Server.

It is important to note that your work machine should have MySQL installed. Please refer to Chapter 1, App Architecture and Development Environment, for instructions.

You should also be familiar with the following technologies:

- TypeScript
- Node.js and npm
- SQL (including basic concepts such as tables, columns and their types, relationships, and primary and foreign keys)
- GraphQL basics (including resolvers, queries, and mutations)

If you want to learn more about SQL, please see https://www.khanacademy.org/computing/computer-programming/sql.

You should also go through TypeORM's official documentation, which is accessible at https://typeorm.io/#/

If you need a review on the fundamentals of GraphQL, please see https://graphql.org/learn/.

The code for this chapter may be found in the GitHub repository at https://github.com/PacktPublishing/Full-Stack-App-Development-with-Angular-and-GraphQL/tree/main/Chapter03.

## 创建 MySQL 用户和数据库

When we first installed MySQL on our development machine, it created a root user who can manage the database system. This user has complete access to the MySQL server, including all databases and tables.

Furthermore, beginning with MySQL v5.7, the root user can be authenticated via the auth socket plugin rather than a username and password, as allowed by the MySQL client used with TypeORM.

> **Important Note**
> 
> The name of the system user that launches the MySQL client and the MySQL user must be the same when using the auth socket plugin; moreover, you must execute mysql with sudo privileges to have access to your database(s).

For the reasons stated above, it is required to create a new user in MySQL and provide it with the right permissions. To do so, go through the following steps:

1. To begin, use the following command to connect to MySQL:
2. Once you've accessed the MySQL prompt, you may use SQL commands such as CREATE USER to create a new user with a password that utilizes the mysql native password plugin for authentication:
3. We must provide it with the necessary permissions using the following command:
4. Then, use the following command to reload all of the privileges:

```
sudo mysql
```

You'll be asked to enter a password. Enter your system's root password.

```
CREATE USER 'dbuser'@'localhost' IDENTIFIED WITH mysql_native_password BY 'p4ssw0rd';
```

In this case, we created a user called dbuser with the password p4ssw0rd.

```
GRANT ALL PRIVILEGES ON *.* TO dbuser@'localhost';
```

We have given the user all of the privileges (that is, full root access to everything in our database). This is not advised for security reasons. However, keep in mind that this is simply a test database.

```
FLUSH PRIVILEGES;
```



5. 退出当前的 MySQL 提示符：

```
exit;
```

6. 使用以下命令以新用户身份登录：

```
mysql -u dbuser -p
```

输入分配给用户的密码，换句话说，p4ssw0rd。

7. 最后，要创建数据库，请使用以下命令：

```
create database socialdb;
```

就这样！我们有一个名为 dbuser 的用户，密码为 p4ssw0rd，以及一个 socialdb 数据库。这些凭据将在下一节中用于设置 TypeORM。

## 设置 TypeORM 和 MySQL

在本节中，我们将在服务器项目中安装和配置 TypeORM。

TypeORM 是一个 Node.js 和 TypeScript 对象关系映射器（ORM）。它让你能够使用 TypeScript 以及类、装饰器和函数等高级编程结构来构建和查询数据库表，而不是使用 SQL 命令。

返回到你的终端。要启动服务器，请确保你在服务器项目中，然后执行以下命令：

```
cd ngsocial/packages/server/
npm start
```

确认你位于服务器项目内部。之后，在另一个终端中执行以下命令：

```
cd ngsocial/packages/server/
npm install typeorm reflect-metadata mysql
```

reflect-metadata 库对于提供 TypeORM 所需的 TypeScript 装饰器支持至关重要。

接下来，打开 tsconfig.json 文件并进行以下更改：

```
{
"compilerOptions": {
"target": "es6",
"module": "commonjs",
"rootDir": "./src",
"outDir": "./dist",
"esModuleInterop": true,
"strict": true,
"experimentalDecorators": true,
"emitDecoratorMetadata": true,
"strictPropertyInitialization": false
}
}
```

我们添加了 experimentalDecorators 和 emitDecoratorMetadata 属性并将其设置为 true。我们还添加了 strictPropertyInitialization 并将其设置为 false。

由于装饰器目前仍是实验性功能，experimentalDecorators 属性为其提供实验性支持。emitDecoratorMetadata 属性告诉编译器发出装饰声明所需的元数据。为了生成装饰器元数据，我们必须在 TypeScript 代码中导入 reflect-metadata 包。

strictPropertyInitialization 属性指示编译器是否应在类中启用或禁用严格属性初始化检查。

然后，打开 src/index.ts 文件并开始修改导入，如下所示：

```
import express, { Application } from 'express';
import { ApolloServer } from 'apollo-server-express';
import schema from './graphql/schema';
import cors from 'cors';
import 'reflect-metadata';
import { createConnection, Connection } from 'typeorm';
```

然后，按如下方式编辑剩余代码：

```
const connection: Promise<Connection> = createConnection();
connection.then(() => {
    startApolloServer();
}).catch(error => console.log("Database connection error: ", error));
```

```ts
async function startApolloServer() {
  const PORT = 8080;
  const app: Application = express();
  app.use(cors());
  const server: ApolloServer = new ApolloServer({ schema });
  await server.start();
  server.applyMiddleware({
    app,
    path: '/graphql'
  });
  app.listen(PORT, () => {
    console.log('Server is running at http://localhost:${PORT}');
  });
}
```

为了支持装饰器，我们导入了 reflect-metadata 库。随后，我们从 TypeORM 包中导入了 createConnection 函数，并删除了几个符号，例如 context，在本部分中不再需要。

reflect-metadata 包是一个实用库，可增强 TypeScript 装饰器的功能。这对于 TypeORM 正常运行至关重要。

然后丢弃了 mocks 对象，并更改了创建 Apollo Server 的行以移除 mocks 参数。在本部分中我们不需要 mocks，因为我们将创建实际的解析器。

我们使用 createConnection 函数创建了 TypeORM 连接，然后调用了返回的 promise 的 then() 方法。接下来，在 promise 的 then() 方法体中，我们调用了构建 Express 和 Apollo Server 的代码。这确保了只有在成功连接到 MySQL 数据库后服务器才会启动。

如果出现问题，包含错误详细信息的数据库连接错误消息将被打印到控制台。

由于我们在之前的步骤中已经启动了服务器，在保存文件后我们会收到 Database connection error: TypeORMError: No connection options were found in any orm configuration files 错误。

为了解决此问题，使用以下命令在 server/ 文件夹中创建 ormconfig.json 文件：

```
touch ormconfig.json
```

然后，在 ormconfig.json 文件中添加以下内容：

```
{
  "type": "mysql",
  "host": "localhost",
  "port": 3306,
  "username": "dbuser",
  "password": "p4ssw0rd",
  "database": "socialdb",
  "synchronize": true,
  "logging": false,
  "entities": [
    "src/entity/**/*.ts"
  ],
  "migrations": [
    "src/migration/**/*.ts"
  ],
  "subscribers": [
    "src/subscriber/**/*.ts"
  ]
}
```

这是 TypeORM 配置文件，我们在其中添加数据库连接配置设置，例如数据库类型（在我们的实例中为 mysql）、数据库主机和端口、数据库名称、用户名和密码。

之后，如果一切配置正确，使用以下命令停止并重新启动开发服务器：

```
npm start
```

因此，我们现在已经配置了 TypeORM。接下来，我们将介绍如何为我们的社交应用创建数据库实体。

## 创建 TypeORM 实体

在本部分，我们将创建代表社交网络数据库的实体。实体是一个用 @Entity() 装饰器注解的 TypeScript 类，对应于数据库表。

当使用纯 SQL 而非 ORM 处理数据库时，你首先创建数据库表。使用 TypeORM 时，我们也必须首先创建实体。

要创建实体，请将 @Entity() 装饰器添加到类定义中，并将 @Column（或等效）装饰器添加到类的每个属性中。这些注解为 TypeORM 提供了创建数据库表和列所需的信息。

我们还可以使用以下装饰器在实体之间建立关联：

- OneToOne: This is used to create a one-to-one relationship between two objects.
- JoinColumn: This specifies which part of the relationship contains the join column.
- OneToMany: This specifies a one-to-many relationship between objects.
- ManyToOne: This specifies a many-to-one relationship between objects.
- ManyToMany: This is used to establish a many-to-many relationship among objects.
- JoinTable: This specifies the join columns of a many-to-many relationship.

通过运行以下命令在 src/ 文件夹中创建一个名为 entity/ 的子目录：

```
mkdir src/entity
```

由于我们已根据应用需求定义了 GraphQL 模式类型，因此我们可以根据模式结构简单地考虑数据库结构。

因此，在我们的情况下，以下实体是必要的：

- User
- Post
- Comment
- Like
- Notification

要定义实体，我们必须首先在 entity/ 文件夹下创建一个 TypeScript 文件。基本实体由一个或多个列组成，其中一个应为主列。

首先创建 src/entity/User.ts 文件，然后添加以下实体：

```
import { Entity, Column, PrimaryGeneratedColumn, CreateDateColumn } from 'typeorm';

@Entity()
export class User {
    @PrimaryGeneratedColumn() id: number;
    @Column() fullName: string;
    @Column("text", { nullable: true }) bio: string;
    @Column({ unique: true }) email: string;
    @Column({ unique: true }) username: string;
    @Column() password: string;
    @Column({ nullable: true }) image: string;
    @Column({ nullable: true }) coverImage: string;
    @Column({ default: 0 }) postsCount: number;
    @CreateDateColumn() createdAt: Date;
}
```

如前所述，@Entity() 指示 TypeORM 将类映射到数据库表。为了让 TypeORM 更容易发现实体，我们将其放在 entity/ 文件夹中。这已在 ormconfig.json 文件的 entities 数组中预先配置。

为了生成匹配数据库的表列，我们只需使用 @Column 装饰器装饰希望作为列的类属性即可。

默认情况下，TypeORM 会将字符串映射为类似 varchar 的类型（取决于数据库类型），将数字映射为类似 integer 的类型。我们也可以通过向列装饰器提供所需类型来修改默认类型。值得注意的是，我们对 bio 列使用了 text 类型而不是受限的 varchar 类型。

TypeORM 将使用与实体相同的名称构建数据库表；但是，如果你想使用不同的表名，可以在 @Entity 装饰器中指定——例如，@Entity("mytablename")。



@PrimaryGeneratedColumn() 装饰器定义了一个主键 Int 列，该列会自动生成自增的值。

我们为 postsCount 列添加了 default 选项，指定其默认值为零；为 bio、image 和 coverImage 列添加了 nullable 选项，指定它们可以为 null；以及为 username 和 email 列添加了 unique 选项，指定不接受重复值。

@CreateDateColumn() 装饰器会自动将实体的插入日期添加到 createdAt 列。

id 列，其中包含相应数据库表的主键，以及 createdAt 列，其中包含插入日期，将会被所有实体共享。

为了构建下一个实体，我们将使用相同的装饰器和参数。

为了表示用户的帖子，创建 src/entity/Post.ts 文件并添加以下实体：

```ts
import { Entity, Column, PrimaryGeneratedColumn,
CreateDateColumn } from 'typeorm';

@Entity()
export class Post {
    @PrimaryGeneratedColumn() id: number;
    @Column("longtext") text: string;
    @Column({ nullable: true }) image: string;
    @Column({ default: 0 }) commentsCount: number;
    @Column({ default: 0 }) likesCount: number;
    @Column({ default: "" }) latestLike: string;
    @CreateDateColumn() createdAt: Date;
    @Column({ default: false }) likedByAuthUser: boolean;
}
```

在这种情况下，我们为 text 列添加了 longtext 选项，以便将列类型从默认值（字符串字段的 varchar(255)）修改为 longtext。

接下来，创建 src/entity/Comment.ts 文件并添加以下实体来建模用户的评论：

```ts
import { Entity, Column, PrimaryGeneratedColumn,
CreateDateColumn } from 'typeorm';

@Entity()
export class Comment {
    @PrimaryGeneratedColumn() id: number;
    @Column("text") comment: string;
    @CreateDateColumn() createdAt: Date;
}
```

为了表示用户的点赞，创建 src/entity/Like.ts 文件并添加以下实体：

```ts
import { Entity, PrimaryGeneratedColumn, CreateDateColumn }
from 'typeorm';

@Entity('likes')
export class Like {
    @PrimaryGeneratedColumn() id: number;
    @CreateDateColumn() createdAt: Date;
}
```

在 Entity 装饰器中，我们使用了 likes 作为表名，而不是 like 的名称，因为 like 是 SQL 中的保留关键字。

最后，将以下实体添加到 src/entity/Notification.ts 文件中以建模用户的通知：

```ts
import { Entity, Column, PrimaryGeneratedColumn,
CreateDateColumn } from 'typeorm';

@Entity()
export class Notification {
    @PrimaryGeneratedColumn() id: number;
    @Column() text: string;
    @Column() postId: number;
    @CreateDateColumn() createdAt: Date;
}
```

此部分的代码可在 GitHub commit at https://git.io/JzddQ 中找到。

接下来，我们必须根据以下要求在实体之间构建某些关系：

- 用户可能拥有一个或多个通知、帖子、评论和点赞。
- 帖子可能与一个或多个评论和点赞、作者以及最近的评论相关联。

返回 src/entity/User.ts 文件并包含以下导入：

```ts
import { OneToMany } from 'typeorm';
import { Post } from './Post';
import { Comment } from './Comment';
import { Like } from './Like';
import { Notification } from './Notification';
```

我们刚刚导入了将与用户实体关联的实体以及在此情况下用于构建一对多关系的装饰器。

接下来添加以下字段：

```ts
@Entity()
export class User {
  // [...] 
  @OneToMany(type => Post, post => post.author) posts:
    Post[];
  @OneToMany(type => Comment, comment => comment.author)
    comments: Comment[];
  @OneToMany(type => Like, like => like.user) likes:
    Like[];
  @OneToMany(type => Notification, notification =>
    notification.user) notifications: Notification[];
}
```

在这里，我们添加了一些额外的数组类型字段，包括 posts、comments、likes 和 notifications，并使用 @OneToMany 注解。这是由 TypeORM 提供的实体关系之一。

使用实体关系时，TypeORM 会在表之间插入必要的外键。

我们建模了用户可能拥有许多评论、点赞和通知的需求。

此外，我们利用了 @OneToMany 装饰器在两个实体之间构建一对多关系。

type => Post 语句是一个返回关联实体类的函数。由于语言限制，采用这种方法而不是直接使用类。() => Post 也是另一个选项。

返回 src/entity/Post.ts 文件并插入以下导入：

```ts
import { OneToOne, OneToMany, ManyToOne, JoinColumn } from 'typeorm';
import { User } from './User';
import { Comment } from './Comment';
import { Like } from './Like';
```

我们导入了关联的用户、评论和点赞实体以及装饰器，以便创建一对一、一对多和多对一关系。

接下来添加以下字段：

```ts
@Entity()
export class Post {
  // [...] 
  @OneToOne(type => Comment, comment => comment.post, {
    onDelete: 'SET NULL' })
  @JoinColumn() latestComment: Comment;
  @ManyToOne(type => User, user => user.posts, { onDelete:
    'CASCADE' }) author: User;
  @OneToMany(type => Comment, comment => comment.post)
  comments: Comment[];
  @OneToMany(type => Like, like => like.post) likes:
  Like[];
}
```

我们为 author、latest comment、comments 和 likes 添加了额外的字段，并使用 @OneToMany、@ManyToOne 和 @OneToOne 注解。

我们建模了帖子可能拥有多个评论、多个点赞、一个最近评论和一个作者的需求。

为了定义关系列在哪一端持有，我们使用了 @JoinColumn 装饰器。在此示例中为 post 表。这是必不可少的，并且应在关系的仅一端存在。

返回 src/entity/Comment.ts 文件并插入以下导入：

```ts
import { ManyToOne } from 'typeorm';
import { User } from './User';
import { Post } from './Post';
```

在这里，我们导入了想要形成关联的用户和帖子实体，以及多对一装饰器。

接下来添加以下字段：

```ts
@Entity()
export class Comment {
  // [...]
  @ManyToOne(type => User, user => user.comments, {
    onDelete: 'CASCADE' })
  author: User;
  @ManyToOne(type => Post, post => post.comments, {
    onDelete: 'CASCADE' })
  post: Post;
}
```

我们在这里添加了 author 和 post 字段并使用 @ManyToOne 注解。这意味着一个人可以添加许多评论，而一个帖子可以有多个评论。这些是为用户和帖子实体指定的逆关系。

返回 src/entity/Like.ts 文件并包含以下导入：

```ts
import { ManyToOne } from 'typeorm';
import { User } from './User';
import { Post } from './Post';
```

为了定义逆关系，我们导入了用户和帖子实体，以及多对一装饰器。

接下来添加以下字段：

```ts
@Entity('likes')
export class Like {
  // [...] 
  @ManyToOne(type => User, user => user.likes, { onDelete:
    'CASCADE' })
  user: User;
  @ManyToOne(type => Post, post => post.likes, { onDelete:
    'CASCADE' })
  post: Post;
}
```

请注意，我们还为用户和帖子添加了额外的字段，并使用 @ManyToOne 注解来定义我们之前为用户和帖子实体创建的逆关系。

返回 src/entity/Notification.ts 文件并包含以下导入：

```ts
import { ManyToOne } from 'typeorm';
import { User } from './User';
```

在这里，我们导入了想要构建逆关系的用户实体，以及多对一装饰器。

然后，包含以下字段：

```ts
@Entity()
export class Notification {
  // [...] 
  @ManyToOne(type => User, user => user.notifications, {
    onDelete: 'CASCADE' })
  user: User;
}
```

我们使用 @ManyToOne 注解添加了通知所属用户的字段。这是用户实体中定义的关系的逆关系。

在此实体中，我们建模了一个或多个通知可能属于一个用户的需求。

添加 TypeORM 关系 的代码可在 GitHub commit at https://git.io/JzdFl 中找到。



因此我们已经构建了建立社交应用数据库所需的实体，以及它们之间的关联。接下来，我们将介绍如何使用测试数据填充数据库表，这将使我们在下一节中能够轻松测试解析器的功能。

## 填充测试数据

到目前为止，我们已经构建了数据库实体，让 TypeORM 构建 SQL 表。现在让我们向数据库中输入一些信息。你可以手动完成，但这会很耗费精力；我们需要足够自动化，以便能够快速启动运行。

我们可以通过利用 typeorm-seeding 包为我们的实体创建工厂和种子器来实现，这简化了流程。请执行以下步骤：

- 1. 返回你的终端，并使用以下命令开始安装该包：
- 2. 安装 Faker 库的类型定义并创建 src/database/、database/seeds/ 和 database/factories/ 文件夹：
- 3. 在 package.json 文件中添加以下脚本以调用种子命令：
- 4. 在 package.json 文件中添加以下命令，以便更轻松地删除和同步数据库：

```
npm install typeorm-seeding
```

```
npm install -D @types/faker
```

```
cd src/ && mkdir database
```

```
cd database && mkdir seeds factories
```

```
"scripts": {
  "seed:config": "ts-node ./node_modules/typeorm-seeding/dist/cli.js config",
  "seed:run": "ts-node ./node_modules/typeorm-seeding/dist/cli.js seed",
  [...] 
}
```

```
"scripts": {
  "schema:drop": "ts-node ./node_modules/typeorm/cli.js schema:drop",
  "schema:sync": "ts-node ./node_modules/typeorm/cli.js schema:sync",
  [...] 
}
```

为了创建并用测试数据填充数据库，会使用一个种子类。它有一个名为 run 的单一方法，我们可以在其中使用 TypeORM 的查询构建器或实体工厂向数据库中输入数据。

为了生成数据，我们可以使用实体工厂，而不是为每个实体种子单独提供属性。

对于每个实体，我们需要创建一个工厂，在其中可以创建实体并返回它。这些工厂文件应创建在 src/database/factories 文件夹中，并以 .factory.ts 为后缀：

- 1. 创建用户实体的工厂。创建 src/database/factories/user.factory.ts 文件并添加以下代码：

```ts
import Faker from 'faker';
import { define } from 'typeorm-seeding';
import { User } from '../../entity/User';

define(User, (faker: typeof Faker) => {
    const user = new User()
    user.fullName = faker.name.findName();
    user.bio = faker.lorem.sentences();
    user.email = faker.internet.email();
    user.username = faker.internet.userName();
    user.password = faker.internet.password();
    user.image = faker.image.imageUrl();
    user.coverImage = faker.image.imageUrl();
    user.postsCount = 200;
    user.createdAt = faker.date.past();
    return user;
});
```

- 2. 在 src/database/factories/post.factory.ts 文件中为 post 实体创建工厂：

```
import Faker from 'faker';
import { define, factory } from 'typeorm-seeding';
import { Post } from '../../entity/Post';

define(Post, (faker: typeof Faker) => {
  const post = new Post();
  post.text = faker.lorem.text();
  post.image = faker.image.imageUrl();
  post.commentsCount = 100;
  post.likesCount = 200;
  post.latestLike = faker.name.findName();
  post.createdAt = faker.date.past();
  return post;
});
```

- 3. 使用以下代码在 src/database/factories/comment.factory.ts 文件中创建 comment 工厂：

```
import Faker from 'faker';
import { define, factory } from 'typeorm-seeding';
import { Comment } from '../../entity/Comment';

define(Comment, (faker: typeof Faker) => {
  const comment = new Comment();
  comment.comment = faker.lorem.text();
  comment.createdAt = faker.date.past();
  return comment;
});
```

- 4. 按如下方式在 src/database/factories/like.factory.ts 文件中创建 like 工厂：

```
import Faker from 'faker';
import { define } from 'typeorm-seeding';
import { Like } from '../../entity/Like';

define(Like, (faker: typeof Faker) => {
    const like = new Like();
    like.createdAt = faker.date.past();
    return like;
});
```

- 5. 按如下方式在 src/database/factories/notification.factory.ts 文件中创建 notification 工厂：

```
import Faker from 'faker';
import { define } from 'typeorm-seeding';
import { Notification } from '../../entity/Notification';

define(Notification, (faker: typeof Faker) => {
    const notification = new Notification();
    notification.text = faker.lorem.words();
    notification.postId = 1;
    notification.createdAt = faker.date.past();
    return notification;
});
```

之后，你必须在 src/database/seeds/create-users.seed.ts 文件中创建一个种子类：

- 6. 首先包含以下导入：

```
import { Seeder, Factory } from 'typeorm-seeding';
import { User } from '../../entity/User';
import { Post } from '../../entity/Post';
import { Comment } from '../../entity/Comment';
import { Like } from '../../entity/Like';
import { Notification } from '../../entity/Notification';
```

- 7. 按如下方式定义种子类：

```
export default class CreateUsers implements Seeder {
    public async run(factory: Factory): Promise<void> {
    }
}
```

- 8. 在 run 方法内部，添加以下代码以使用用户工厂的 createMany 方法创建 15 个用户：

```
await factory(User)().map(async (user: User) => {
  return user;
}).createMany(15);
```

- 9. 在 map 方法内部，添加以下代码以创建 1 到 10 之间的随机数量的评论：

```
const comments: Comment[] = await factory
  (Comment)().map(async (comment: Comment) => {
    comment.author = await
      factory(User)().create();
    return comment;
  }).createMany(Math.floor(Math.random() * 10)
  + 1);
```

- 10. 同样地，创建 1 到 10 之间的随机数量的点赞：

```
const likes: Like[] = await
  factory(Like)().map(async (like: Like) => {
    like.user = await
      factory(User)().create();
    return like;
  }).createMany(Math.floor(Math.random() * 10)
  + 1);
```

- 11. 接下来，创建 1 到 10 之间的随机数量的帖子，并使用以下代码将之前创建的评论和点赞关联到这些帖子：

```
const userPosts: Post[] = await
  factory(Post)().map(async (post: Post) => {
    post.comments = comments;
    post.likes = likes;
    post.latestComment = await
      factory(Comment)().map(async (comment:
        Comment) => {
        comment.author = await
          factory(User)().create();
        return comment;
      }).create();
    return post;
  }).createMany(Math.floor(Math.random() * 10)
  + 1);
```

- 12. 创建通知并按如下方式将其与帖子和当前用户关联：

```
const postIds = userPosts.map((post: Post)
=> post.id);
const notifications: Notification[] = await
factory(Notification)().map(async
(notification: Notification) => {
const postId: number = postIds.pop() as
number;
notification.postId = postId;
return notification;
}).createMany(postIds.length);
```

- 13. 最后，按如下方式在 map 主体中将帖子和通知与当前用户关联：

```
user.posts = userPosts;
user.notifications = notifications;
return user;
```

就这样！这个类将负责生成所有必要的假数据并在它们之间关联条目，就像在现实场景中一样。

之后，我们需要执行以下命令来启动种子器：

```
npm run seed:run
```

本节的代码可在 https://git.io/JzdbJ 的提交中找到。更多信息，请查看 https://www.npmjs.com/package/typeorm-seeding。

现在我们已经用一些测试数据填充了数据库，让我们实现解析器。

## 使用 Apollo 与 TypeORM

在我们创建了实体并插入了一些测试数据后，我们将研究如何将 Apollo 与 TypeORM 集成。在本节中，我们将定义我们的 GraphQL 解析器，这些解析器是在向我们的 GraphQL 端点发送查询和变更时将被调用的函数：

- 1. 首先，你需要在 src/graphql/schema.graphql 文件中，将我们 GraphQL 模式的 Post、Comment 和 Like 类型上的 user 和 author 字段改回 User 类型而不是 ID：
- 2. 此外，将 Comment 和 Like 类型中 post 字段的类型从 ID 改为 Post：

```
author: User!
user: User!
```

```
post: Post!
```

在实现模式解析器之前，我们需要创建类型以确保解析器代码的类型安全。幸运的是，我们不必手动完成。相反，我们可以使用 GraphQL Code Generator，这是一个使用简单 CLI 从你的 GraphQL 模式和操作生成代码的工具。有关此工具的更多信息，请参阅 https://www.graphql-code-generator.com/。

由于这些类型是生成的代码，我们将把它们放在自己的包中，然后从我们的解析器中导入它们。

接下来，我们将研究如何使用 Lerna 创建一个包并配置它以能够导入生成的类型：

- 1. 如果你的服务器仍在运行，请停止它并执行以下命令来创建 graphql-types 包：

```
cd ../../
lerna create graphql-types
```

请注意，你将被询问有关你的包的一些问题。接受默认值并按 Enter。

一个新的 graphql-types 包将在 ./packages/graphql-types 中创建。



现在我们项目中已经有多个包（带有 package.json 文件的文件夹），让我们使用 npm workspaces（由 npm v7+ 支持）来高效地管理我们 monorepo 项目中所有包的依赖关系。

- 2. 只需在 ngsocial/package.json 根文件中添加 workspaces 键，以告知 npm 我们的包所在位置：

```
{
  "name": "@ngsocial/ngsocial",
  "private": true,
  "devDependencies": {
    "lerna": "^4.0.0"
  },
  "version": "0.0.1",
  "workspaces": ["./packages/*"]
}
```

请注意，我们还将根包的名称更改为 @ngsocial/ngsocial 以创建作用域包。有关更多信息，请参阅 https://docs.npmjs.com/cli/v7/using-npm/scope。

Lerna 和 npm 现在都可以在 ./packages 文件夹中找到包。如果许多包需要相同的依赖项，得益于 npm workspaces，它可以在根目录中安装一次，然后从其他包中导入而不会出现问题。

继续我们的步骤：

- 1. 按如下所示在我们 monorepo 项目的根目录中安装 GraphQL Codegen 和所需的插件：

```
npm install @graphql-codegen/cli @graphql-codegen/typescript @graphql-codegen/typescript-resolvers
```

值得注意的是，我们也可以在 graphql-types 包内安装这些依赖项；然而，官方文档建议在根目录中安装 Codegen。有关更多详细信息，请参阅 https://www.graphql-code-generator.com/docs/getting-started/development-workflow。

2. 创建 ngsocial/codegen.yml 文件并添加以下内容：

```
schema: './packages/server/src/graphql/schema.graphql'
generates:
  ./packages/graphql-types/src/resolvers-types.ts:
    plugins:
      - typescript
      - typescript-resolvers
```

3. 打开 ngsocial/package.json 文件并添加以下脚本：

```
"scripts": {
  "codegen": "npx graphql-codegen --watch"
},
```

此脚本将以监视模式启动 GraphQL Codegen。

然后，使用以下命令启动它：

```
npm run codegen
```

这将创建一个包含我们模式解析器所需所有类型的文件 graphql-types/src/resolvers-types.ts。

由于这是一个包含在另一个包中的 TypeScript 文件，我们必须能够将其导入到位于不同包中的服务器代码中。为此，我们可以使用具有适当设置的 tsconfig.json 文件和一个 index.ts 文件。

4. 创建 packages/graphql-types/tsconfig.json 文件并添加以下内容：

```
{
  "compilerOptions": {
    "outDir": "lib",
    "rootDir": "src",
    "baseUrl": "./",
    "composite": true
  },
  "include": ["src/**/**"],
  "exclude": ["node_modules", "**/__tests__/**"]
}
```

"composite": true 选项是必需的，以便能够从其他包中引用此包。

接下来，创建 packages/graphql-types/src/index.ts 文件并添加以下代码：

```
export * from './resolvers-types';
```

这确保了我们导出所需的所有符号。

接下来，你需要打开 graphql-types/package.json 文件并将 name 更改为 @ngsocial/graphql 以创建作用域包。

- 5. 最后，你需要打开 server/tsconfig.json 文件并按如下方式更新它：

```
{
  "compilerOptions": {
    [...] 
    "paths": {
      "@ngsocial/graphql": ["../graphql-types/src"]
    }
  },
  "references": [{ "path": "../graphql-types" }]
}
```

你需要确保 paths 对象中指定的路径名与 graphql-types/package.json 文件中的名称匹配，在我们的例子中是 @ngsocial/graphql。

就这样！我们现在可以通过编写以下内容导入生成的 GraphQL 类型：

```
import { Resolvers, User, QueryGetUserArgs } from '@ngsocial/graphql';
```

为了确保项目各包中命名的一致性，请编辑 server/package.json 文件并将名称更改为 @ngsocial/server。

你可以在 GitHub 上查看这些步骤的更新，网址为 https://git.io/Jzd77。

接下来，让我们执行几个更多步骤：

1. 创建 server/src/entity/index.ts 文件并添加以下导入：

```
export { User } from './User';
export { Post } from './Post';
export { Comment } from './Comment';
export { Like } from './Like';
export { Notification } from './Notification';
```

我们没有从各自的文件导入每个实体，而是添加了一个 barrel 文件，允许我们使用简短路径导入实体，如下所示：

```
import { User, Post, Comment, Like, Notification } from './entity';
```

有关 barrel 文件的更多信息，请参阅 https://basarat.gitbook.io/typescript/main-1/barrel。

2. 打开 server/src/index.ts 文件并添加突出显示的导入：

```
import { createConnection, Connection, Repository, getRepository } from 'typeorm';
```

```
import { User, Post, Comment, Like, Notification } from './entity';
```

我们将使用 typeorm 包中的 Repository 和 getRepository() 符号来操作我们的实体。

在 TypeORM 中，repository 是一个抽象层，提供了一组用于查询和保存实体到数据库的操作。有关更多信息，请查看 https://typeorm.io/#/working-with-repository 和 https://typeorm.io/#/entities。

每个实体都有一个包含用于操作该实体的方法的 repository。可以使用 getRepository() 方法创建 repository，然后传递实体，如下所示：

```
userRepository = getRepository(User);
```

3. 按如下方式定义并导出 Context 类型：

```
export type Context = {
  orm: {
    userRepository: Repository<User>;
    postRepository: Repository<Post>;
    commentRepository: Repository<Comment>;
    likeRepository: Repository<Like>;
    notificationRepository: Repository<Notification>;
  };
};
```

此类型将用于为传递给解析器的 context 对象提供类型。此时，我们只传递一个包含用于操作数据库的 TypeORM repositories 的 orm 对象。

- 4. 在 startApolloServer() 方法中，紧跟在 app.use(cors()) ; 行下方添加以下代码：

```
    const userRepository: Repository<User> =
        getRepository(User);
    const postRepository: Repository<Post> =
        getRepository(Post);
    const commentRepository: Repository<Comment> =
        getRepository(Comment);
    const likeRepository: Repository<Like> =
        getRepository(Like);
    const notificationRepository:
        Repository<Notification> =
        getRepository(Notification);
```

接下来，定义 Context 类型的 context 对象并向其中添加 TypeORM repositories，如下所示：

```
    const context: Context = {
        orm: {
            userRepository: userRepository,
            postRepository: postRepository,
            commentRepository: commentRepository,
            likeRepository: likeRepository,
            notificationRepository: notificationRepository
        }
    };
```

5. 最后，如以下代码所示，替换创建 Apollo Server 时提供 context 和 schema 的代码行：

```ts
const server: ApolloServer = new ApolloServer({ schema,
  context });
```

为了与数据库协作，我们现在可以在解析器中使用此 context 对象以及提供的 repositories。我们也可以选择在解析器中为每个实体调用 getRepository() 函数，但出于多种原因（包括易于测试），这是首选解决方案。你可以在 https://git.io/JzdQM 的提交中查看之前步骤的更改。

接下来，我们将为我们的 GraphQL 模式实现解析器：

1. 打开 server/src/graphql/resolvers.ts 文件并按如下方式更新导入：

```ts
import { Context } from '...';
import {
  Resolvers,
  User,
  Post,
  Comment,
  Like,
  Notification } from '@ngsocial/graphql';
import { ApolloError } from 'apollo-server-errors';
```

2. 将 resolvers 对象的类型更新为 Resolvers 而不是 IResolvers：

```ts
const resolvers: Resolvers = {
  Query: {
    message: () => 'It works!'
  }
};
export default resolvers;
```

因此，我们所有的模式解析器及其参数都将是类型安全的。




我们利用自定义的 Context 类型作为解析器的上下文对象，因为它使用 any 进行类型化。我们也可以告诉 GraphQL Codegen 使用此类型作为上下文。有关更多详细信息，请参阅 https://the-guild.dev/blog/better-type-safety-for-resolvers-with-graphql-codegen。

接下来，在 Query 对象内部，逐一添加下面列出的解析器函数，用逗号分隔：

- 1. 首先包含 getUser 查询解析器，用于通过 ID 查找用户：

```ts
getUser: async (_, args, ctx: Context) => {
    const orm = ctx.orm;
    const user = await orm
        .userRepository
        .findOne({
            where:
                { id: args.userId }});
    if (!user) {
        throw new ApolloError(
            "No user found",
            "USER_NOT_FOUND");
    }
    return user as unknown as User;
}
```

为了找到指定 ID 的用户，我们利用从解析器上下文传递的 orm 对象中获取的用户存储库的 findOne() 方法。如果未找到用户，则会抛出 Apollo 错误。

在返回之前，我们将来自 User 实体 的 user 对象转换为 User 类型，并且由于 TypeScript 警告类型重叠不足，因此我们首先将对象转换为 unknown 类型。

- 2. 实现 getPostsByUserId 解析器以根据用户 ID 检索帖子：

```ts
getPostsByUserId: async (_, args, ctx: Context) => {
    const posts = await ctx.orm.postRepository
        .createQueryBuilder("post")
        .where({ author: { id: args.userId } })
        .leftJoinAndSelect("post.author", "post_author")
        .leftJoinAndSelect("post.latestComment",
    "latestComment")
        .leftJoinAndSelect("latestComment.author",
    "latestComment_author")
        .leftJoinAndSelect("post.likes", "likes")
        .leftJoinAndSelect("likes.user", "likes_user")
        .orderBy("post.createdAt", "DESC")
        .skip(args.offset as number)
        .take(args.limit as number)
        .getMany();
    return posts as unknown as Post[];
}
```

TypeORM 中的查询生成器允许您创建和执行基本和高级 SQL 查询。在这种情况下，post 是所选帖子的别名。别名用于访问所选数据的列。

有关如何使用查询生成器的更多信息，请参阅 https://typeorm.io/#/select-query-builder。

3. 实现 getFeed 解析器以获取所有网络用户帖子的动态：

```ts
getFeed: async (_, args, ctx: Context) => {
    const feed = await ctx.orm.postRepository
        .createQueryBuilder("post")
        .leftJoinAndSelect("post.author", "post_author")
        .leftJoinAndSelect("post.latestComment",
            "latestComment")
        .leftJoinAndSelect("latestComment.author",
            "latestComment_author")
        .leftJoinAndSelect("post.likes", "likes")
        .leftJoinAndSelect("likes.user", "likes_user")
        .orderBy("post.createdAt", "DESC")
        .skip(args.offset as number)
        .take(args.limit as number)
        .getMany();
    return feed as unknown as Post[];
}
```

4. 实现 getNotificationsByUserId 解析器，用于根据其 ID 检索属于用户的通知：

```ts
getNotificationsByUserId: async (_, args, ctx: Context) => {
  const notifications = await
    ctx.orm.notificationRepository
    .createQueryBuilder("notification")
    .innerJoinAndSelect("notification.user", "user")
    .where("user.id = :userId", { userId: args.userId })
    .orderBy("notification.createdAt", "DESC")
    .skip(args.offset as number)
    .take(args.limit as number)
    .getMany();
  return notifications as unknown as Notification[];
}
```

5. 实现 getCommentsByPostId 解析器以根据其 ID 获取帖子的评论：

```ts
getCommentsByPostId: async (_, args, ctx: Context) => {
  return await ctx.orm.commentRepository
    .createQueryBuilder("comment")
    .innerJoinAndSelect("comment.author", "author")
    .innerJoinAndSelect("comment.post", "post")
    .where("post.id = :id", { id: args.postId as string })
    .orderBy("comment.createdAt", "DESC")
    .skip(args.offset as number)
    .take(args.limit as number)
    .getMany() as unknown as Comment[];
}
```

6. 实现 getLikesByPostId 解析器以根据其 ID 获取帖子的点赞：

```ts
getLikesByPostId: async (_, args, ctx: Context) => {
    return await ctx.orm.likeRepository
        .createQueryBuilder("like")
        .innerJoinAndSelect("like.user", "user")
        .innerJoinAndSelect("like.post", "post")
        .where("post.id = :id", { id: args.postId })
        .orderBy("like.createdAt", "DESC")
        .skip(args.offset as number)
        .take(args.limit as number)
        .getMany() as unknown as Like[];
}
```

7. 实现 searchUsers 解析器以按姓名或用户名搜索用户：

```ts
searchUsers: async (_, args, ctx: Context) => {
    const users = await ctx.orm.userRepository
        .createQueryBuilder("user")
        .where('user.fullName Like'
            '%${args.searchQuery}%')
        .orWhere('user.username Like'
            '%${args.searchQuery}%')
        .getMany();
    return users as unknown as User[];
}
```

现在我们已经实现了查询，让我们继续进行突变。将以下方法添加到 Mutation 对象中：

1. 首先包含以下解析器：

```ts
post: (_, args, ctx: Context) => {
    throw new ApolloError("Not implemented yet",
        "NOT_IMPLEMENTED_YET");
},
comment: (_, args, ctx: Context) => {
    throw new ApolloError("Not implemented yet",
        "NOT_IMPLEMENTED_YET");
},
like: (_, args, ctx: Context) => {
    throw new ApolloError("Not implemented yet",
    "NOT_IMPLEMENTED_YET");
},
removeLike: async (_, args, ctx: Context) => {
    throw new ApolloError("Not implemented yet",
    "NOT_IMPLEMENTED_YET");
}
```

我们在这里只是抛出一个 Apollo 错误，表示该解析器在本章中尚未实现。我们将在下一章中实现它们，在实现身份验证后，因为我们需要有关当前（已认证）用户的信息。

- 2. 实现 removePost 解析器以通过其 ID 从数据库中删除帖子，如下所示：

```ts
removePost: async (_, args, {orm}: Context) => {
    const post = await orm.postRepository.findOne(
        args.id, { relations: ['author'] });
    if (!post) {
        throw new ApolloError("Post not found",
            "POST_NOT_FOUND");
    }
    const result: DeleteResult = await
        orm.postRepository.createQueryBuilder()
        .delete().from(PostEntity).where("id = :id", { id:
            args.id }).execute();
    const postsCount = post?.author?.postsCount;
    if (postsCount && postsCount >= 1) {
        await orm.userRepository.update({ id:
            post?.author.id }, { postsCount: postsCount - 1
        });
    }
    if (result.affected && result.affected <= 0) {
        throw new ApolloError("Post not deleted",
            "POST_NOT_DELETED");
    }
    return args.id;
}
```

有关更多详细信息，请查看 https://typeorm.io/#/delete-query-builder。

3. 实现 removeComment 解析器以通过其 ID 从数据库中删除评论：

```ts
removeComment: async (_, args, {orm}: Context) => {
    const comment = await orm.commentRepository.findOne(
        args.id, {relations:['author', 'post']});
    if (!comment) {
        throw new ApolloError("Comment not found",
            "COMMENT_NOT_FOUND");
    }

    const result: DeleteResult = await
        orm.commentRepository.delete(args.id);
    if (result.affected && result.affected <= 0) {
        throw new ApolloError("Comment not deleted",
            "COMMENT_NOT_DELETED");
    }

    const commentsCount = comment?.post?.commentsCount;
    if (commentsCount && commentsCount >= 1) {
        await orm.postRepository.update(comment.post.id, {
            commentsCount: commentsCount - 1 });
    }

    return comment as unknown as Comment;
}
```

4. 实现 removeNotification 解析器以通过其 ID 从数据库中删除通知：

```ts
removeNotification: async (_, args, {orm}: Context) => {
    const notificationRepository =
        orm.notificationRepository;
    const notification = await
```

```
notificationRepository.findOne(args.id,
    {relations:['user']});
if(!notification){
    throw new ApolloError("Notification not found",
        "NOTIFICATION_NOT_FOUND");
}
const result: DeleteResult = await
    notificationRepository.delete(args.id);
if(result.affected && result.affected <= 0) {
    throw new ApolloError("Notification not deleted",
        "NOTIFICATION_NOT_DELETED");
}
return args.id;
}
```

值得注意的是，我们没有包含创建通知的 mutation。这是因为它们必须在人们评论或点赞内容时创建。因此，我们可以在每次新增评论或点赞实体时简单地创建它们。

我们可以通过在 Comment 和 Like 实体某些方法上使用 AfterInsert 装饰器来实现，该装饰器将在实体插入数据库后被调用：

- 1. 打开 server/src/entity/Comment.ts 文件并添加以下导入：
- 2. 向类中添加 createNotification() 方法并使用 AfterInsert 装饰器对其进行注解，如下所示：

```
import { AfterInsert, getRepository } from 'typeorm';
import { Notification } from './Notification';
```

```
@AfterInsert()
async createNotification() {
    if (this.post && this.post.id) {
        const notificationRepository =
            getRepository(Notification);
        const notification =
            notificationRepository.create();
```

```
notification.user = await getRepository(
    User).createQueryBuilder("user")
    .innerJoinAndSelect("user.posts", "post")
    .where("post.id = :id", { id: this.post.id })
    .getOne() as User;
    notification.postId = this.post.id;
    notification.text = '${this.author.fullName}
    commented on your post';
    await notificationRepository.save(notification);
  }
}
```

使用通知存储库的方法，我们在该方法内创建必要的通知。

然后我们必须在 Like 实体上重复此过程：

- 1. 向 server/src/entity/Like.ts 文件添加以下导入：

```
import { AfterInsert, getRepository } from 'typeorm';
import { Notification } from './Notification';
```

- 2. 添加一个 @AfterInsert 装饰的方法，其中你必须实现代码以提供合适的通知，告知用户有人点赞了他们的帖子：

```
@AfterInsert()
async createNotification() {
  if (this.post && this.post.id) {
    const notificationRepository =
      getRepository(Notification);
    const notification =
      notificationRepository.create();
    notification.user = await getRepository(
      User).createQueryBuilder("user")
      .innerJoinAndSelect("user.posts", "post")
      .where("post.id = :id", { id: this.post?.id })
      .getOne() as User;
    notification.postId = this.post?.id;
    notification.text = '${this.user.fullName} liked
      your post';
```

```
await notificationRepository.save(notification);
}
}
```

使用存储库的 create() 方法，我们构建了一个 Notification 对象并填充了必要的属性，例如用户、帖子 ID 和文本。此外，我们利用了 TypeORM 的查询构建器来获取应接收通知的用户，即创建该帖子的用户。

你可以使用 Apollo Studio 来尝试我们构建的解析器。如果你的服务器已经运行，可以前往 https://studio.apollographql.com/sandbox/explorer 开始发送查询和变更：

- 1. 如果你发送 post、comment、like 或 removeLike 中的一个 mutation，你应该会收到包含 Not implemented yet 文本和 NOT_IMPLEMENTED_YET 代码的错误消息：

```
mutation {
  post(text: "Hello World"){
    id
  }
  comment(comment: "This is a comment", postId: 100 )
  {
    id
  }
  like(postId: 100) {
    id
  }
  removeLike(postId: 100) {
    id
  }
}
```

要发送 mutation，你需要使用 mutation 操作类型，然后指定要发送的一个或多个 mutation。更多信息请参考 https://graphql.org/learn/queries/。

- 2. 你可以单独或同时发送以下任何查询。首先发送 getFeed 查询以检索网络上发布的帖子列表：

```
query {
  getFeed(offset: 0 limit: 10) {
    id text commentsCount likesCount author { id
    fullName }
  }
}
```

在此情况下我们必须使用 query 操作类型。我们使用模式的 getFeed 字段向服务器请求帖子 feed。我们传入 offset 和 limit 变量以指示服务器仅提供可用帖子的子集，而不是全部，并且我们利用选择集来请求服务器所需的子字段。为了从嵌套的 author 对象中获取所需字段，我们还必须使用选择集。

- 3. 你可以然后取一个用户 ID 并将其传递给以下查询以获取用户的帖子。在我的例子中：

```
getPostsByUserId(userId:224 limit: 5) {
  id text author { id fullName } createdAt
}
```

- 4. 要访问用户的信息，例如他们的全名、简介和帖子数，运行以下查询：

```
getUser(userId: 224) {
  id fullName postsCount bio
}
```

- 5. 你可以通过运行以下查询为特定用户获取通知：

```
getNotificationsByUserId(userId: 19) {
  id text createdAt
}
```

- 6. 使用帖子 ID 和以下查询，你可以检索特定帖子的点赞和评论：

```
getCommentsByPostId(postId: 10 limit: 5) {
  id comment author { id fullName } post { id }
}
```

```
}
    getLikesByPostId(postId: 17 limit: 5) {
      id user { id fullName } post { id }
    }
```

- 7. 最后，你可以通过执行以下 mutation 删除帖子、评论和通知：

```
mutation {
  removePost(id: 59)
  removeComment(id: 21) {
    id
  }
  removeNotification(id: 10)
}
```

在此情况下，我们发送 mutation 删除 ID 为 59 的帖子、ID 为 21 的评论以及 ID 为 10 的通知。

我们必须为 removeComment mutation 使用选择集，因为该 mutation 返回被删除的评论对象。其他两个 mutation 返回被删除的帖子或通知的 ID（标量）。

我们收到的响应如下：

```
{
  "data": {
    "removePost": "59",
    "removeComment": {
      "id": "21"
    },
    "removeNotification": "10"
  }
}
```

这表明我们成功删除了这些对象。

## 摘要

在本章中，我们学习了如何使用 TypeORM 将 MySQL 数据库连接到我们的应用程序，以及如何实现一些解析器从数据库获取和删除数据。这使我们能够编写一个功能性的 GraphQL API，其解析器从真实的 MySQL 数据库获取数据。

我们使用 TypeORM 来抽象数据库操作，这允许你在使用应用程序时使用任何选择的数据库管理系统而无需更改代码。

我们在将 TypeORM 与 Apollo 集成之前研究了如何使用初始数据生成和填充数据库表。

本章到此结束！在下一章中，我们将继续使用 Apollo Server 和 Node.js 构建后端应用程序，以实现身份验证和图像上传。
