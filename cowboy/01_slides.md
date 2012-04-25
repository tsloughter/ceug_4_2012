!SLIDE bullets
# Erlang Apps #

* Webmachine or Cowboy for REST
* JSX or Jiffy for JSON
* Maru for extras

!SLIDE center
# Cowboy Example #

!SLIDE code
# Dispatch #

    @@@ Erlang
    {[<<"todos">>], bcmvc_todo_handler, []}
    {[<<"todos">>, todo], 
        bcmvc_todo_handler, []}

!SLIDE code
# Authorize #

    @@@ Erlang
    init(_Transport, _Req, _Opts) ->
      {upgrade, protocol, cowboy_http_rest}.

    allowed_methods(Req, State) ->
      {['HEAD', 'GET', 'PUT', 'POST', 
          'DELETE'], Req, State}.

    content_types_accepted(Req, State) ->
      {[{{<<"application">>, <<"json">>, []}, 
          put_json}], Req, State}.

    content_types_provided(Req, State) ->
      {[{{<<"application">>, <<"json">>, []}, 
          get_json}], Req, State}.


!SLIDE code
# POST #

    @@@ Erlang
    process_post(Req, State) ->
      {ok, Body, Req1} = 
          cowboy_http_req:body(Req),
      Todo = 
          bcmvc_web_utils:qs_to_proplist(Body),   
      TodoModel = bcmvc_model_todo:new(Todo),
      bcmvc_model_todo:save(TodoModel),
      {true, Req1, State}.

!SLIDE code
# PUT #

    @@@ Erlang
    put_json(Req, State) ->
      {ok, Body, Req1} = 
          cowboy_http_req:body(Req),
      {TodoId, Req2} = 
          cowboy_http_req:binding(todo, Req1),
      Todo = bcmvc_model_todo:set(
          bcmvc_model_todo:to_record(Body), 
              id, TodoId),
      bcmvc_model_todo:update(Todo),    
      {true, Req2, State}.

!SLIDE code
# GET #

    @@@ Erlang
    get_json(Req, State) ->
     All = [bcmvc_model_todo:to_json(Model) 
           || Model <- bcmvc_model_todo:all()]
     JsonModels = list_to_json(All),
     {<<"[", JsonModels/binary, "]">>, 
          Req, State}.

!SLIDE code
# DELETE #

    @@@ Erlang
    delete_resource(Req, State) ->
      {TodoId, Req1} = 
          cowboy_http_req:binding(todo, Req),
      bcmvc_model_todo:delete(TodoId),
      {true, Req1, State}.
