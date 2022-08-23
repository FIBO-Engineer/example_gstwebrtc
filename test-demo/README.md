######################
# Fork the code from gstwebrtc-demo to test webrtc between jetson nano (gstwebrtc, CSI camera) and web browser 
# Jetson nano CSI camera not support mediadevices getusermedia 
######################

# Generate key following self-signed-certificate-read-me.md
# Copy all file that generated from openssl to web and signalling folder

# Web server and signalling must run in the same machine otherwise you need to revise code

# In web folder run command
# npm install
# npm start 

# In signalling folder run command to start websocker server for signalling 
# python3 -m pip install --user websockets
# python3 simple_server.py

# try to open browser https://ipaddress:8080
# It should display Status: Registered with server, waiting for call

# Copy Jetson nano folder into jetson nano board (with CSI camera) file system
# Install gstreamer (The python version requires at least version 1.14.2 of gstreamer and its plugins.) 
# apt-get install -y libjson-glib-dev
# python3 -m pip install --user websockets
# run python3 sendrecv/gst/webrtc_sendrecv.py ID with the id from the browser. You will see state changes and an SDP exchange.