Using this script will get the flag

	from pwn import * # pip install pwntools
	from Crypto.Util.number import long_to_bytes
	import json
	import base64
	import codecs
	import array

	r = remote('socket.cryptohack.org', 13377, level = 'debug')
	i = 0
	def json_recv():
    		line = r.recvline()
    		return json.loads(line.decode())

	def json_send(hsh):
   		 request = json.dumps(hsh).encode()
    		r.sendline(request)

	while i<=101 :
		received = json_recv()

		print("Received type: ")
		print(received["type"])
		print("Received encoded value: ")
		print(received["encoded"])

		if(received["type"] == "base64") :
			base64_message = received["encoded"]
			base64_bytes = base64_message.encode('ascii')
			decoded_bytes = base64.b64decode(base64_bytes)
			decoded = decoded_bytes.decode('ascii')
		elif(received["type"] == "hex") :
			bytes_object = bytes.fromhex(received["encoded"])
			decoded = bytes_object.decode("ASCII")
		elif(received["type"] == "rot13") :
			decoded = codecs.decode(received["encoded"],"rot13")
		elif(received["type"] == "bigint") :
			bigint = int(received["encoded"],16)
			decoded_bytes = long_to_bytes(bigint)
			decoded = decoded_bytes.decode('ascii')
		elif(received["type"] == "utf-8") :
			decoded_bytes = array.array('B', received["encoded"]) 
			decoded = codecs.decode(decoded_bytes,"utf-8")
		
	
	


	

		to_send = {
	    		"decoded": decoded
		}
		json_send(to_send)
		i = i+1
	
