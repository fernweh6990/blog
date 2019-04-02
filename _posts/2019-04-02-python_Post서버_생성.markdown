---
layout: post
title:  "python으로 Post 서버 만들기"
date:   2019-04-02 20:20:00
categories: Python post server
---

 
  
   
### python으로 Post 서버 만들기
#### Post request를 수신하고, json response 하는 서버

서버간 통신하는 API를 개발하는 중에, 상대 서버와 아직 통신할 수 없는 상황이었다.
준비가 되기 전에 우선 개발을 진행하기 위해서 특정 값을 응답하는 서버를 생성할 필요가 있었다.



``` python
from http.server import HTTPServer, BaseHTTPRequestHandler

from io import BytesIO


class SimpleHTTPRequestHandler(BaseHTTPRequestHandler):


    def do_POST(self):
        demo_data = '''
         [
         {
         "data1": "aaa",
         "data2": "111",
         "data3": "xxx"
         },
         {
         "data1": "bbb",
         "data2": "222",
         "data3": "yyy"
         }
         ]
        '''

        content_length = int(self.headers['Content-Length'])
        body = self.rfile.read(content_length)
        self.send_response(200)
        self.send_header("Content-Type","application/json;charset=UTF-8")
        self.end_headers()
        response = BytesIO()

        response.write(str.encode(demo_data))
        self.wfile.write(response.getvalue())

 # 8000 포트로 서버 동작
httpd = HTTPServer(('localhost', 8000), SimpleHTTPRequestHandler)
httpd.serve_forever()



```

