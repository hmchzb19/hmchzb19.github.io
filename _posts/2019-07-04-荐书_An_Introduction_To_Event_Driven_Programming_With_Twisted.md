这个书刚开始看，翻了第一章感觉是在教你如何写类似twisted这样的框架，感觉不错。  
异步asyncio 和twisted 对我来说是很复杂的，总感觉是对应c里面的select/epoll  
就是多路复用。  

但是有些地方略晕,  
python3的non-block 会触发的exception 改成了BlockingIOError。  
python2里面是socket.error  
但我试了下在try exception里面这两种exception 都可以。  
`except BlockingIOError as e:`
或者  
`except socket.error as e:`

另外，该书在github上的代码不全。  
我自己敲了第一章20页的完整代码， 
该代码会触发MemoryError.  

````
import select

class Reactor:
    def __init__(self):

        self._readers = {}
        self._writers = {}
    
    def addReader(self, readable, handler):
        self._readers[readable]= handler
    
    def addWriter(self, writable, handler):
        self._writers[writable]=handler
    
    def removeReader(self, readable):
        self._readers.pop(readable, None)
    
    
    def removeWriter(self, writable):
        self._writers.pop(writable, None)
    
    def run(self):
        while self._readers or self._writers:
            r, w, _ = select.select(list(self._readers), list(self._writers),[])
            for readable in r:
                self._readers[readable](self, readable)
            
            for writable in w:
                if writable in self._writers:
                    self._writers[writable](self, writable)

import errno
import socket

class BuffersWrites:
    def __init__(self, dataToWrite, onCompletion):
        self._buffer = dataToWrite
        self._onCompletion = onCompletion
    
    def bufferingWrite(self, reactor, sock):
        if self._buffer:
            try:
                written = sock.send(self._buffer)
            except socket.error as e:
                if e.errno != errno.EAGAIN:
                    raise 
                return 
            else:
                print("Wrote", written, ' bytes')
                self._buffer = self._buffer[written:]
        if not self._buffer:
            reactor.removeWriter(sock)
            self._onCompletion(reactor, sock)


def accept(reactor, listener):
    server, _ = listener.accept()
    #server.setblocking(False)
    reactor.addReader(server, read)

def read(reactor, sock):
    data = sock.recv(1024)
    if data:
        print("Server received", len(data), ' bytes')
    else:
        sock.close()
        print("Server closed")
        reactor.removeReader(sock)
        

DATA=[b'*', b'*']
def write(reactor, sock):
    writer = BuffersWrites(b''.join(DATA), onCompletion=write)
    reactor.addWriter(sock, writer.bufferingWrite)
    print('Client buffering', len(DATA), ' bytes to write.')
    DATA.extend(DATA)

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.bind(('127.0.0.1', 0))
sock.listen(1)
client= socket.create_connection(sock.getsockname())
client.setblocking(False)

loop = Reactor()
loop.addWriter(client, write)
loop.addReader(sock, accept)
loop.run()
````