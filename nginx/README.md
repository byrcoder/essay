# nginx ssl config

# compile
./auto/configure --with-stream_ssl_module  --with-stream 

# conf
example conf:

stream {

  map_hash_max_size 262;
  map_hash_bucket_size 262;

  map $ssl_server_name $targetBackend {
    1.ap-mumbai.mediapackage.srclivepull.myqcloud.com  127.0.0.1:9000;
    2.ap-mumbai.mediapackage.srclivepull.myqcloud.com  127.0.0.1:9001;
  }

  map $ssl_server_name $targetCert {
    1.com /usr/local/nginx/certs/1.92B87E13.crt;
    2.com /usr/local/nginx/certs/2.crt;
  }

  map $ssl_server_name $targetCertKey {
    1.com /usr/local/nginx/certs/1.key;
    2.com /usr/local/nginx/certs/2.key;
  }

  server {
    listen 443 ssl;
    ssl_protocols       TLSv1.2;
    ssl_certificate     $targetCert;
    ssl_certificate_key $targetCertKey;

    proxy_connect_timeout 1s;
    proxy_timeout 3s;

    proxy_pass $targetBackend;
  }
}


