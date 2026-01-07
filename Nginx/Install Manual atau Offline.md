
Download Nginx installer https://nginx.org/en/download.html

Extract Nginx di dalam VM lokal.
Compile Nginx agar siap dipakai di server. Lakukan :

1. Masuk ke dalam direktori Nginx (dalam kasus ini lokasi di /opt/nginx/).
2. Buat direktori logs. Buat files error.log dan access.log di dalam direktori tersebut.
3. Jalankan script configure
```
   ./configure \
	--prefix=/opt/nginx \
	--sbin-path=/opt/nginx/sbin/nginx \
	--conf-path=/opt/nginx/conf/nginx.conf \
	--error-log-path=/opt/nginx/logs/error.log \
	--http-log-path=/opt/nginx/logs/access.log \
	--pid-path=/opt/nginx/logs/nginx.pid \
	--with-pcre \
	--with-http_ssl_module \
	--with-http_v2_module
	
	make
	sudo make install
```

4. Jadikan tar.
```
tar -czvf nginx-compiled.tar nginx/
```
5. Copy dan Ekstrak di VM Server. Buat Servicenya.
```
	nano /etc/systemd/system/nginx.service

	[Unit]
	Description=The NGINX HTTP and reverse proxy server
	After=syslog.target network-online.target remote-fs.target nss-lookup.target
	Wants=network-online.target

	[Service]
	Type=forking
	PIDFile=/opt/nginx/logs/nginx.pid

	# Path ke file binary di /opt/nginx/sbin/nginx
	ExecStartPre=/opt/nginx/sbin/nginx -t
	ExecStart=/opt/nginx/sbin/nginx
	ExecReload=/opt/nginx/sbin/nginx -s reload
	ExecStop=/bin/kill -s QUIT $MAINPID
	PrivateTmp=true

	[Install]
	WantedBy=multi-user.target
```
