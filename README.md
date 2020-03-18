# snorqfy_app
Snorqfy APP

Pre-requisite:

brew install mkcert
brew install nss # if you use Firefox

mkcert -key-file key.pem -cert-file cert.pem example.com "*.example.com" example.test localhost 127.0.0.1 ::1