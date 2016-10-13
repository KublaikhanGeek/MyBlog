title: Running NGINX in the Foreground
date: 2015-11-18 14:27:58
tags: [nginx]
categories: nginx
---

## (copy) Running NGINX in the Foreground

copying from [http://honeyco.nyc/blog/running-nginx-in-the-foreground/](http://honeyco.nyc/blog/running-nginx-in-the-foreground/)

默认情况下nginx是以daemon方式启动的，下面是如何让nginx在前台运行  

If you’re planning on using nginx has a frontend to one or more app processes, it can be helpful to setup and test things locally. An easy way to do this is to run nginx in the foreground so you can quickly iterate your config options to see what works. First, you need to create a config:

<!--more-->


	# nginx.conf
	# put wherever you want, mine is in the base of my project directory

	worker_processes  1;
	daemon off;

	events {
		worker_connections 1024;
	}

	http {
		server {
			listen 8080;
		}
	}

the most important line is `daemon off` which will cause nginx to run in the foreground. This will still run with the typical master / worker setup though, and can be used in production.

The rest is the most minimal boilerplate to get nginx to serve an http server on port 8080.

Try opening browsing to http://localhost:8080/. You should see a successful installation message from nginx. From here you can setup proxies to your apps or whatever other config you want to test out.  


You can also specify this on the command-line via nginx -g "daemon off;"
