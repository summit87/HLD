#### Websocket

* When to use

    * For bi directional communication . (Live chat, gaming , etc..)
    * IOT device
    * Getting continously update for trading

* Which protocol used by Websocket

    * TCP (This is persitence connection , i.e once TCP connection open it will remains open until user session or user active in that TCP connection)

* How tcp connection will get establish : Below step to establish tcp connection b/w client and server

    * User send HTTP request
    * Once server receive the request and do the TCP handshak
    * Once TCP handshake is success , then server return HTTP status 101 i.e server will tell to client now i am switching communication
    protocol , i,e Websocket.
* How web socket is different from HTTP sticky session / long-polling / polling

    * Http stickey session / keep-alive

        * <b>HTTP stcikey session : </b> Http srtickey session will be started when we sent <b>coonection : keep-alive</b>
        * TCP connection will remains open . TCP connectoin will close in only two way's
            
            * Either user close the session or we can setup timeout in header while creating connection as keep-alive

        * Keep-alive will work as <b>request - response </b>. i.e Client still need to make request to get the data. Bidirectional communication (Where server will push the data to client) is not supported.
        * When we have very frequent TCP opening / closing kind of request , then we can use keep-alive.
        * Once TCP connection is open , for each request and response , TCP connection handshaking will not be done ,due to this , Network I/O will reduce , which improve performance in network end

        * Only for each TCP handshake, SSL termination will be done , once TCP connection will establish , SSL termincation will not done , in that we are reducing CPU over head in server 

    * Http polling

        * Once client sent request and server retrun the response, TCP connection closed , for other request similar process will happen , In that case , for each request we have to establish TCP connection, which will increse CPU overhead in server

    * HTTP long polling

        * Once client sent request , server keep connection open untill data is not ready from server , in case of timeout connection will close, then again client need to send the request for server for data . 

        * This will establish the TCP persistence connection

        * Resource / CPU utilization will be more or in case of timeout resource will be wastefull .

        * Unnecessary we are holding the transaction for longer time for no use.


* How to scale Websocket : Since its connection type is persistence in nature with TCP Scaling websocket is very trickey . </br> If we are scaling then , we need to distribut the mesasge accross the server for consistency

    