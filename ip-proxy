const https = require('https');
const http = require('http');

const TARGET_HOST = '47.253.206.60';
const TARGET_PORT = 443;

const server = http.createServer((req, res) => {
    const options = {
        hostname: TARGET_HOST,
        port: TARGET_PORT,
        path: req.url,
        method: req.method,
        headers: { ...req.headers, host: TARGET_HOST },
        rejectUnauthorized: false
    };

    const proxyReq = https.request(options, (proxyRes) => {
        res.writeHead(proxyRes.statusCode, proxyRes.headers);
        proxyRes.pipe(res);
    });

    proxyReq.on('error', (err) => {
        res.writeHead(502);
        res.end('Proxy error: ' + err.message);
    });

    req.pipe(proxyReq);
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
    console.log(`Proxy running on port ${PORT}`);
});
