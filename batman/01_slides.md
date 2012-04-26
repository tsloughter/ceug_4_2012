!SLIDE center
# JS MVC Advantages #

![Trash](someone_else.png)

!SLIDE bullets
# JS MVC Advantages #

* Backend developer safety!
* Nothing new to learn for frontend devs
* No waiting on updates to template engine
* Node.js of fixture prototypes by frontend dev

!SLIDE center
# Batman.js Example #

![Batman.js](batmanjs.png)

!SLIDE center
# TodoMVC #
![TodoMVC](todomvc.png)

!SLIDE code
# Batman.js Structure #

    @@@ Ruby
    ├── coffee
    │   └── app.coffee
    ├── controllers
    │   └── todos_controller.coffee
    ├── index.html
    ├── models
    │   └── todo.coffee
    
!SLIDE code
# View #

    @@@ html
    <li data-foreach-todo="Todo.all"
      data-event-doubleclick="controllers.todos.edit"
	  data-addclass-completed="todo.isDone">
      <div class="view">
        <input class="toggle" 
          type="checkbox" data-bind="todo.isDone" />
        <label data-bind="todo.body" />
	    <button class="destroy" 
          data-event-click="todo.destroy"></button>
	  </div>
      <input data-bind-id="todo.id" class="edit"
        data-bind-value="todo.body" 
        data-event-submit="controllers.todos.update">
	</li>
    

!SLIDE code
# Model #

    @@@ coffeescript
    class TodoMVC.Todo extends Batman.Model
      @persist TodoMVC.JSONRestStorage
      @encode 'body', 'isDone'

      body: ''
      isDone: false


!SLIDE code
# Controller #

    @@@ coffeescript
    class TodoMVC.TodosController extends Batman.Controller
        index: ->
            @set 'emptyTodo', new Todo
            @render false

        create: =>
          @todo.save =>
            @set 'emptyTodo', new Todo

        edit: (node, event) ->
          $(node).addClass('editing')

        update: (node, event) ->
          newTodo = new Todo({id: $(node).attr('id'), body: $(node).val()})
          newTodo.save()
          $(node).parent().removeClass('editing')

!SLIDE code
# API #

    @@@ javascript
    POST:
    /todos/
    {body:"bane wants to meet, not worried",isDone:false}
    
    PUT:
    /todos/33e93b30-2371-4071-afc5-2d48226d5dba
    {body:"bane wants to meet, not worried",isDone:false}
    
    GET:
    /todos/
    [{id:"33e93b30-2371-4071-afc5-2d48226d5dba",
      body:"bane wants to meet, not worried",
      isDone:false}]
    
    DELETE:
    /todos/33e93b30-2371-4071-afc5-2d48226d5dba
