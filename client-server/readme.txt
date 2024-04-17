    server 						client
      |							  |
    socket()					        socket()
      |							  |
    bind()						connect()
      |							  |
    listen()					          |
      |							  |
    accept()					          |
      |							  |
      |							  |
    read() <------------------------------------------ write()
      |							  |
      |							  |
    wrie() ------------------------------------------> read()
      |							  |
      |							  |
    close()						close()



client - 
	socket : API is used for creating socket from client
	connect: on connect call client sends request to server and waits for server to accept it
	send : Clinet sends data packet to server
	read : client reads packet sent by server
	close : closes connection with server
	
server -
	socket : API is used for creating socket from server
	bind   : 
	listen :
	accept :
	read   :
	send   :
	close  : closes connection with server
	
