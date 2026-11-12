
## Adverts

There are many advert providers, and each provides its own libraries for Flutter. The recommended one for mobile is Google's own Mobile Ads SDK, which Google provides an official library for. This is easy to integrate, and you will make money by showing adverts to the players within your game.

Be aware that overuse of adverts will annoy players, so please try and think of the experience the user will get while playing the game to try and balance this out.

Let's look at the pros and cons of using adverts in your game:

## Pros:

- Easy to integrate.
- Repeat income from a constant revenue stream.

## Cons:

- Can be annoying for users.
- Money paid for adverts is quite low.

Now, let's look at another monetization option, in-app purchases.

## In-app purchases

In-app purchases can be a great way to make money from your game. Generally, with this type of game, you give it away free and then take small micropayments in the game, which work through Google Play or Apple's App Store. You are purchasing virtual goods that only exist in that game. For instance, in our game, you can purchase extra lives or a better weapon to use in exchange for real money.

Be aware that some existing games that use in-app purchases have a reputation for paying to win, where the user that is willing to pay to buy the best items in the game can win the game easily. This is controversial, especially in multiplayer games where you can buy success in the game.

From our view as developers, virtual goods provide an interesting way to monetize our games. This is because the game assets are virtual, and they can be sold many times to different players and provide a reliable, repeatable revenue stream.

Let's look at the pros and cons of using in-app purchases in your game:

## Pros:

- You can sell the same virtual goods to many different users.

## Cons:

- It can be fiddly to set up in the mobile developer portals.
- It can be irritating to users if overused.

Now that we have discussed the pros and cons of in-app purchases, let's look at our final option, purchase.

## Purchase

One-time purchases are also another valid option for monetizing your app. You can set a fixed price for your game that the users pay once.

Be aware that players tend to want to see updated content in the game, such as new levels. So, if you only make money from a fixed price, you will be maintaining the game for free, aside from any new sales you may make by updating the content.

Let's look at the pros and cons of using purchases in your game.

## Pros:

- It's hassle free, and you only have to set the price once.

## Cons:

- There's no repeat income from existing customers.

If you want to further read up on ways to monetize your game, please check out https://flutter.dev/monetization, which goes into more detail.

In this section, we discussed ways of monetizing your game. In the next section, we will discuss what else is worth learning to expand your game development knowledge.

## What else should I learn?

Now you have mastered the basics of game development with Flame, let's look at what else you can learn that are more advanced topics but are very useful in expanding your game development knowledge.

## Forge2d

A lot of games use advanced physics to make the games more realistic, such as using gravity to affect how sprites jump and fall or calculating the trajectory of a falling bird in Angry Birds.

There is a very good physics engine available called Forge2d, which is based on the famous Box2d engine, which is worth investigating if you want to make your games more realistic.

You can find everything you need to get started at https://docs.flame-engine.org/1.0.0/forge2d.html.

## Nakama

Single-player games can be a lot of fun, but games go to a whole new level when you play them with your friends. Multiplayer game development is a very complex subject to do from scratch, but it is a useful subject to learn and can improve your games a lot. Fortunately, Flutter has a good library that handles all the complex stuff needed to build multiplayer games, which works well with Flame.

The library is called **Nakama**, which is discussed at https://heroic-labs.com/docs/nakama/getting-started/, and the Flutter library for this is at https://github.com/obrunsmann/flutter_nakama.

Nakama has many features. Here are a few of the things it can do:

- Multiplayer gaming between different players
- Real-time chat between players
- Leaderboards for tracking who has the highest score
- User accounts with logins

Nakama covers many more multiplayer features than we cover here, so it is recommended that you check out their website (https://heroic-labs.com/docs/nakama/getting-started/) for more information.

## Rive

**Rive** is a cross-platform animation tool that allows you to export an animation and play it back, using libraries specific to each platform. If you are familiar with the old Flash animations, this is similar.

You can read more about Rive at https://rive.app/. The Flame library for Rive can be found at https://pub.dev/packages/flame_rive.

## What games shall I make?

Flame is suitable for all types of 2D games, so you can make whatever game you like. The difficulty is that some types of games are more complicated to code than others.

If you plan on making role-playing games, which are very popular, be aware that these are very complicated games to make, as they often simulate real-world mechanics and require you to build a lot of content for the game.

Start simple with games such as tic-tac-toe and build up to games such as Sokoban or Breakout, before moving on to platform games such as Mario.

There is a great article at https://gamefromscratch.com/just-starting-out-what-games-should-i-make/ to give you some more ideas about what games to make and in what order you should make them.

In the next section, we will discuss where to get help when you get stuck with Flutter/Flame.

## Where to get help?

There will be times when you are making your game that you will get stuck. You may not know how to implement something or there might be a bug with the Flame library or Flutter SDK, for instance.

Here, I have compiled a list of great resources where you can seek help if you get stuck:

- Flame Discord (https://discord.gg/5unKpdQD78): Here, you can ask the Flame development team questions and get excellent advice on how to use Flame.

We especially want to thank Spydon, Erick, and Wolfen on the Flame Discord server for all the help they gave while I worked on the book.

- Stack Overflow: The same people who monitor Flame Discord monitor Stack Overflow, where you can also post questions. Be sure to tag your questions with the #flame tag.
- Flame documentation: The Flame documentation can be found at https://pub.dev/documentation/flame/latest/, and you can find many examples and tutorials for Flame at https://github.com/flame-engine/flame/tree/main/examples and https://github.com/flame-engine/awesome-flame#articles--tutorials.
- Flutter community page: For more general Flutter help, please check out the Flutter community page at https://flutter.dev/community, where you can find links to the Flutter Discord and Slack channels.

In this section, we discussed where to get help when you get stuck with Flame or Flutter.

## Summary

In this chapter, we learned how to mix our game widget with other Flutter widgets and navigate between screens to complete our game. We also gave the player the option to change the music volume on the settings screen.

We discussed options for monetizing your game and what to learn next to improve your game development knowledge. Finally, we reviewed where to get help if you get stuck with your game.

We have covered a lot in the book, and you now have the knowledge to build a variety of 2D games with Flutter and Flame. Good luck with your game development journey. I look forward to seeing the games that you create.

## Questions
