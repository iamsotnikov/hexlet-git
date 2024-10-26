from http.server import SimpleHTTPRequestHandler, HTTPServer
import os

class CustomHandler(SimpleHTTPRequestHandler):
    def do_GET(self):
        # Указываем путь к index.html
        file_path = '/data/index.html'
        
        if os.path.exists(file_path):
            # Если файл существует, читаем его содержимое
            with open(file_path, 'r') as file:
                content = file.read()
            
            # Отправляем успешный HTTP ответ
            self.send_response(200)
            self.send_header('Content-type', 'text/html')
            self.end_headers()
            # Отправляем содержимое файла
            self.wfile.write(content.encode())
        else:
            # Если файл не найден, возвращаем 404
            self.send_response(404)
            self.end_headers()
            self.wfile.write(b'File not found!')

# Настройки сервера
server_address = ('', 8080)
httpd = HTTPServer(server_address, CustomHandler)
print("Server started on port 8080...")
httpd.serve_forever()
