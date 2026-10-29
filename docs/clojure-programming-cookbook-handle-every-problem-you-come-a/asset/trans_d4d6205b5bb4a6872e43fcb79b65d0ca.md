

```
(enable-console-print!)
;;=> nil
```

以下列表是应用程序管理的任务列表。

## 定义状态

在下面的代码中，我们将任务定义为一个映射的 atom：

```
(def task-state
  (atom {:task
          [{:id 1 :task "Writing the om.next ui code" :due-date "12/May" :finished false}
           {:id 2 :task "Testing the om.next ui code" :due-date "10/Jun" :finished false}
           {:id 3 :task "Writing the liberator server code" :due-date "25/Jun" :finished false}
           {:id 4 :task "Testing the liberator server code" :due-date "01/July" :finished false}
           {:id 5 :task "Integrate test with the ui and server" :due-date "15/Aug" :finished false}]})
;;=> #'om-next-example.task/task-state
```

## 定义读取

然后，我们将定义一个读取，它是多方法：

```
(defmulti read-task om/dispatch)
;;=> nil
(defmethod read-task :task
  [env key params]
  (let [state (:state env)]
    {:value (:task @state)}))
;;=> #object[cljs.core.MultiFn]
```

在之前的代码中，env 类似于以下内容：

```
{:query-root :om.next/root,
         ..........
  :children
  [{:type :prop, :dispatch-key :id, :key :id}
   {:type :prop, :dispatch-key :task, :key :task}
   {:type :prop, :dispatch-key :finished, :key :finished}
   {:type :prop, :dispatch-key :due-date, :key :due-date}]},
  :state
  #object [cljs.core.Atom {:val {:task [{:id 1, :task "Writing
```

```
the om.next ui code", :due-date "12/May", :finished true} {:id 2, :task "Testing the om.next ui code", :due-date "10/Jun", :finished true} ....]]}],
:parser
#object[om$next$impl$parser$parser__$self "function
(env,query,target) {
.........
```

而 read-task 返回任务数组：

```
{:value [{:id 1, :task "Writing the om.next ui code", :due-date "12/May", :finished true}
{:id 2, :task "Testing the om.next ui code", :due-date "10/Jun", :finished true}....]}
```

## 定义变异

我们将在下面定义名为 mutate-task 的多方法：

```
(defmulti mutate-task om/dispatch)
;;=> nil
```

变异有三种方法，如下表所示：

| 操作 | 描述 |
|---|---|
| task/new | 用于新建任务的变异 |
| task/toggle | 用于切换任务完成与未完成状态的变异 |
| task/delete | 用于删除任务的变异 |

让我们定义 task/new 的变异。在定义变异之前，我们将定义一个工具函数 get-next-id 来为新任务生成新的 ID：

```
(defn get-next-id [task]
(->>
(map :id task) (apply max) inc))
;;=> #'om-next-example.task/get-next-id
(get-next-id (@task-state :task))
;;=> 6
```

以下方法用于为新任务创建映射并将其插入到 task-state 中：

```
(defmethod mutate-task 'task/new [env key params]
  (let [
    state (:state env)
    id (get-next-id (:task @state))
    new-task (assoc params :id id)]
    {:action
     (fn []
       (swap! state update :task conj new-task))}))
;;=> #object[cljs.core.MultiFn]
```

然后，我们将定义一个方法来在完成/未完成之间切换任务的状态。get-index-for-id 用于获取 id 的数组位置：

```
(defn get-index-for-id
  [id task]
  (-> (for [[index task] (map-indexed vector task)
            :when (= id (:id task))]
        index) first))
;;=> #'om-next-example.task/get-index
```

以下方法在完成/未完成之间切换任务的状态：

```
(defmethod mutate-task 'task/toggle
  [env key params]
  (let [state (:state env)
        id (:id params)
        index (get-index-for-id id (:task @state))]
    {:action
     (fn []
       (swap! state update-in [:task index :finished] not))}))
;;=> #object[cljs.core.MultiFn]
```

最后，我们将定义一个用于删除任务的方法：

```
(defn delete-task
  [task id]
  (-> (remove #(= id (:id %)) task) vec))
;;=> #'om-next-example.task/delete-task
(defmethod mutate-task 'task/delete
  [env key params]
```

```
(let [state (:state env)
      id (:id params)
      task (delete-task (:task @state) id)]
  {:action
   (fn []
     (swap! state assoc :task task))})
;;=> #object[cljs.core.MultiFn]
```

## 定义 UI

我们将为任务应用程序定义两个 UI。它们是 ListView 和 TopView。ListView 与单个任务配合使用，并将它们显示为表格的行。每一行的元素为复选框、任务 ID、任务描述、截止日期和删除按钮。TopView 将任务显示为表格并处理新任务。

让我们先看 ListView：

```
(defui ListView
  static om/IQuery
  (query [this]
    [:id :task :finished :due-date])
  Object
  (render [this]
    (let [{:keys [id task finished due-date]} (om/props this)
      (dom/tr nil
        (dom/th nil
          (dom/input #js
            {:type "checkbox"
             :className "toggle"
             :checked (and finished "checked")
             :onChange #
             (om/transact! this `[(task/toggle {:id ~id})])}))
        (dom/th nil
          (dom/span #js
            {:className "id" :style #js
             {:color (if finished "red" "black")}}
            (if finished (dom/s nil id) id)))
        (dom/th nil
          (dom/span #js
            {:className "task" :style #js
             {:color (if finished "red" "black")}}
```

```
(if finished (dom/s
nil task) task))
(dom/th nil
(dom/span #js
{:className "due-date" :style #js
{:color (if finished "red" "black")}}
(if finished (dom/s
nil due-date) due-date))
(dom/th nil
(dom/button #js
{:onClick #
(om/transact! this `[(task/delete {:id ~id})])}
"delete")))))
;;=> #object[Function "function
(this__47196__auto__,writer__47197__...
```

我们已经为状态定义了查询。查询获取 id、task、due-date 和 finished 的值。

om/factory 从 UI 组件创建函数以供父组件调用：

```
(def list-view (om/factory ListView))
;;=> #'om-next-example.task/list-view
```

因此，让我们继续定义 RootView。我们将定义 get-element-value-by-id 和 set-element-value-by-id!。_get-element-value-by-id 通过 id 从特定输入字段获取值：

```
(defn get-element-value-by-id [id]
(.-value (. js/document (getElementById id))))
;;=> #'om-next-example.task/get-element-value-by-id
```

然而，set-element-value-by-id! 设置输入字段的值：

```
(defn set-element-value-by-id! [id value]
(set! (.-value (. js/document (getElementById id))) value))
;;=> #'om-next-example.task/set-element-value-by-id!
```

我们还将定义 not-white-space? 来检查输入字段是否不是空白：

```
(defn not-white-space? [s]
(not (= (clojure.string/trim s) "")))
```

```
;;=> #'om-next-example.task/not-white-space?
(defn handle-new-task!  [component _]
  (let [new-task (get-element-value-by-id "new-task")
        due-date (get-element-value-by-id "due-date")]
    (when (and (not-white-space? new-task) (not-white-space?
    due-date))
      (om/transact! component
                    `[(task/new ~{:task new-task :due-date due-date})])
      (set-element-value-by-id! "due-date" "")
      (set-element-value-by-id! "new-task" ""))))
;;=> #'om-next-example.task/handle-new-task!
```

现在我们将按如下方式定义 RootView：

```
(defui RootView
  static om/IQuery
  (query [this]
          (let [subquery (om/get-query ListView)]
            [{:task subquery}]))
  Object
  (render [this]
          (let [props (om/props this)
                task (:task props)]
            (dom/div nil
                     (dom/label nil "Your task =>  ")
                     (dom/input #js {:className "new-task" :id "new-task"
                                     :placeholder "Enter your new task"})
                     " / "
                     (dom/input #js {:className "due-date" :id "due-date"
                                     :placeholder "Due date"})
                     (dom/button #js
                                 {:onClick #(handle-new-task! this %)}
                                 "new")
                     (dom/hr nil)
                     (apply dom/table #js {:style #js {:text-align "left"}}
                            (map list-view task))
                     (dom/hr nil)))))
;;=> #object[Function "function (this__47196__auto__)...
```

我们已经定义了 ListView 和 RootView。是时候运行我们的应用程序了。

