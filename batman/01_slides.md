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
# View #

    @@@ html
    <li data-foreach-todo="Todo.all"
    	data-event-doubleclick="controllers.todos.edit"
	    data-addclass-completed="todo.isDone">
    	<div class="view">
	      <input class="toggle" type="checkbox" data-bind="todo.isDone" />
          <label data-bind="todo.body"></label>
	      <button class="destroy" data-event-click="todo.destroy"></button>
	    </div>
        <input data-bind-id="todo.id" class="edit"
            data-bind-value="todo.body" 
            data-event-submit="controllers.todos.update">
	</li>

!SLIDE code
# Model #

    @@@ coffeescript
    class TodoMVC.Todo extends Batman.Model
      @global yes # global exposes this class on the global object, so you can access `Todo` directly.

      @persist Batman.RestStorage # Batman.LocalStorage
      @encode 'body', 'isDone'

      body: ''
      isDone: false


!SLIDE code
# Controller #

    @@@ coffeescript
    class TodoMVC.TodosController extends Batman.Controller
        @todo: null
        
        index: ->
            @render false

        create: =>
          @todo.save =>
            @set 'emptyTodo', new Todo
