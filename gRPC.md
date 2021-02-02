### gRPC

____

- 클라이언트에서 다른 기기에 있는 서버의 메소드를 불러올 수 있음 => Distributed application 및 service를 구축하기 편함
- 장점
  - 서버와 클라이언트 간 통신에 ProtocolBuffer 형식을 이용: 기존의 string type을 이용하는 JSON이나 XML에 비해 용량, 속도 측면에서 효율적
  - .proto 파일에 서비스를 정의해놓으면 gRPC가 지원하는 어려 언어를 사용하는 다양한 플랫폼에서(서버/클라이언트) 동일 서비스를 활용 가능



- 구축하기

  1.  

     ```
     service RouteGuide {	// service name
     	
     	//	rpc methods with request and response types
     	rpc GetFeature(Point) returns (Feature) {} //	1. simple RPC
     	
     	rpc ListFeatures(Rectangle) returns (stream Feature) {} //	2. response-streaming RPC
     	
     	rpc RecordRoute(stream Point) returns (RouteSummary) P{} //	3. request-streaming RPC
     	
     	rpc RouteChat(stream RouteNote) returns (stream RouteNote) {} //	4. bidirectinoally-streaming RPC
     }
     
     message Point {		//	message type definitions for request and response
     	int32 latitude = 1;
     	int32 longitude = 2;
     }
     ```

     - simple RPC: client sends a request to server using stub and waits for the response to come back (like a normal function call)
     - response-streaming RPC: client sends a request to server and gets a stream to read a sequence of messages back, client reads from the returned stream until there are no more messages
     - request-streaming RPC: client writes a sequence of messages and sends them to server using a provided stream. After finishing to write the messages, it waits for the server to read them all and return its reponse
     - both sides send a sequence of messages using a read-write stream. two streams operate independently, so clients and servers can read and write in whatever order they like (ex: the server could wait to receive all the client messages before writing its responses, or it could alternately read a message then write a message, or some other combination of reads and writes) the order of messages in each stream is preserved