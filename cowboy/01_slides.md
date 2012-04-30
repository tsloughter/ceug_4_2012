!SLIDE bullets
# Erlang Apps #

* Webmachine or Cowboy for REST
* JSX or Jiffy for JSON
* Maru for extras

!SLIDE center
# Cowboy Example #

!SLIDE code
<style>
.sh_ruby { line-height: .7; }
code {font-size: 20px; }
</style>
# Structure #

    @@@ Ruby
    ├── lib
    │   ├── bcmvc_db
    │   │   └── src
    │   │       ├── bcmvc_db_app.erl
    │   │       ├── bcmvc_db.app.src
    │   │       ├── bcmvc_db.erl
    │   │       ├── bcmvc_db_mnesia.erl
    │   │       ├── bcmvc_db_sup.erl
    │   │       └── bcmvc_idioms.hrl
    │   ├── bcmvc_models
    │   │   └── src
    │   │       ├── bcmvc_models.app.src
    │   │       ├── bcmvc_model_todo.erl
    │   │       ├── bcmvc_model_transform.erl
    │   │       ├── bcmvc_model_types.erl
    │   │       └── bcmvc_model_utils.erl
    │   └── bcmvc_web
    │       ├── priv
    │       └── src
    │           ├── bcmvc_todo_handler.erl
    │           ├── bcmvc_web_app.erl
    │           ├── bcmvc_web.app.src
    │           ├── bcmvc_web_sup.erl
    │           └── bcmvc_web_utils.erl

!SLIDE code
# Todo Model #
    
    @@@ Erlang
    -compile({parse_transform, bcmvc_model_transform}).
    
    -record(bcmvc_model_todo, {id = ossp_uuid:make(v1, text) :: string(),
                               body                          :: binary(),
                               isDone                        :: boolean()}).
                               
    find(Criteria) when is_list(Criteria) ->
      bcmvc_db:find(?MODULE, Criteria);
    find(Criteria) when is_tuple(Criteria)->
      bcmvc_db:find(?MODULE, [Criteria]).

    to_json(Record) ->
      ?record_to_json(?MODULE, Record).

    to_record(JSON) ->
      ?json_to_record(?MODULE, JSON).


!SLIDE code
# Dispatch #

    @@@ Erlang
    {[<<"todos">>], bcmvc_todo_handler, []}
    {[<<"todos">>, todo], bcmvc_todo_handler, []}

!SLIDE code
# Authorize #

    @@@ Erlang
    init(_Transport, _Req, _Opts) ->
      {upgrade, protocol, cowboy_http_rest}.

    allowed_methods(Req, State) ->
      {['HEAD', 'GET', 'PUT', 'POST', 'DELETE'], Req, State}.

    content_types_accepted(Req, State) ->
      {[{{<<"application">>, <<"json">>, []}, put_json}], Req, State}.

    content_types_provided(Req, State) ->
      {[{{<<"application">>, <<"json">>, []}, get_json}], Req, State}.


!SLIDE code
# POST #

    @@@ Erlang
    process_post(Req, State) ->
      {ok, Body, Req1} = cowboy_http_req:body(Req),
      Todo = bcmvc_model_todo:to_record(Body),
      bcmvc_model_todo:save(Todo),
      
      NewId = bcmvc_model_todo:get(id, Todo),
     {ok, Req2} = cowboy_http_req:set_resp_header(
                   <<"Location">>, <<"/todos/", NewId/binary>>, Req1),

      {true, Req1, State}.

!SLIDE code
# PUT #

    @@@ Erlang
    put_json(Req, State) ->
      {ok, Body, Req1} = cowboy_http_req:body(Req),
      {TodoId, Req2} = cowboy_http_req:binding(todo, Req1),
      Todo = bcmvc_model_todo:set(bcmvc_model_todo:to_record(Body), id, TodoId),
      bcmvc_model_todo:update(Todo),    
      {true, Req2, State}.

!SLIDE code
# GET #

    @@@ Erlang
    get_json(Req, State) ->
     All = [bcmvc_model_todo:to_json(Model) || Model <- bcmvc_model_todo:all()]
     JsonModels = list_to_json(All),
     {<<"[", JsonModels/binary, "]">>, Req, State}.

!SLIDE code
# DELETE #

    @@@ Erlang
    delete_resource(Req, State) ->
      {TodoId, Req1} =  cowboy_http_req:binding(todo, Req),
      bcmvc_model_todo:delete(TodoId),
      {true, Req1, State}.
