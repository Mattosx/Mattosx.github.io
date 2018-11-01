---
title: Hello World
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

``` cpp
static void
server_send_cb(EV_P_ ev_io *w, int revents)
{
    server_ctx_t *server_send_ctx = (server_ctx_t *)w;
    server_t *server              = server_send_ctx->server;
    remote_t *remote              = server->remote;
    if (server->buf->len == 0) {
        // close and free
        close_and_free_remote(EV_A_ remote);
        close_and_free_server(EV_A_ server);
        return;
    } else {
        // has data to send
        ssize_t s = send(server->fd, server->buf->data + server->buf->idx,
                         server->buf->len, 0);
        if (s == -1) {
            if (errno != EAGAIN && errno != EWOULDBLOCK) {
                ERROR("send");
                close_and_free_remote(EV_A_ remote);
                close_and_free_server(EV_A_ server);
            }
            return;
        } else if (s < server->buf->len) {
            // partly sent, move memory, wait for the next time to send
            server->buf->len -= s;
            server->buf->idx += s;
            return;
        } else {
            // all sent out, wait for reading
            server->buf->len = 0;
            server->buf->idx = 0;
            ev_io_stop(EV_A_ & server_send_ctx->io);
            ev_io_start(EV_A_ & remote->recv_ctx->io);
        }
    }
}
```
``` cpp
PythonConnection::PythonConnection(PythonServer *owner, uv_tcp_t *client) : prevStderr_(NULL),
																			prevStdout_(NULL),
																			pClient_(client),
																			owner_(owner),
																			telnetSubnegotiation_(false),
																			historyPos_(-1),
																			charPos_(0),
																			active_(false),
																			multiline_(),
																			softspace_(0)
{
	PyObject_INIT(static_cast<PyObject *>(this), &_typeObject);

	pClient_->data = this;

	unsigned char options[] =
		{
			TELNET_IAC, TELNET_WILL, TELNET_ECHO,
			TELNET_IAC, TELNET_WONT, TELNET_LINEMODE, 0};

	this->write((char *)options);
	this->write("welcome to python console");
	this->write("\r\n");
	this->writePrompt();

	uv_read_start((uv_stream_t *)pClient_, alloc_buffer, PythonConnection::onReadDataWrapper);
}
```
More info: [Deployment](https://hexo.io/docs/deployment.html)
