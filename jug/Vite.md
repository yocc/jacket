# Vite

----

[toc]



## Overviews









## FAQ

```sh
问题: 
4:41:17 AM [vite] http proxy error at /admin/api/stars/get:
Error: getaddrinfo ENOTFOUND dev-cassi.sports.weibo.cn
    at GetAddrInfoReqWrap.onlookup [as oncomplete] (node:dns:107:26) (x5)
  vite:time 16962.08ms /admin/api/stars/get +9s

原因:
dev-cassi.sports.weibo.cn 域名解析不到, 是 vite 这个 web server 服务内部使用的域名解析解析不到, 而不是 vite 所在机器解析不了域名. 
也就是说在 vite 机器上, 是可以 curl 到的, 但在 vite 的 web server 里是 不行.

解决:
/etc/hosts
增加远程 域名 解析配置
10.13.241.26 dev-cassi.sports.weibo.cn


```

```sh
# vite.config.js
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  server: {
    host: '0.0.0.0',
    // port: 5173,
    proxy: {
        "/dev-cassi": {
            // target: 'https://dev-cassi.sports.weibo.cn',
            target: 'http://chenchen.cassi.sports.weibo.cn',
            changeOrigin: true,
            rewrite: (path) => path.replace(/^\/dev-cassi/, ''),
            headers: {
                cookie: 'SUB=_2A25O82efDeRhGedO7VQY9SjMzD-IHXVtid5XrDV8PUNbmtAGLWX-kW9NXQnL4wnqFNi07ocAsMIA95paChP5P3Iq;'
            }
        }
    }
  }
})
```

```sh
# package.json
{
  "name": "fr",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "dev": "vite -d -l info",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.2.45" 
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.0.0",
    "vite": "^4.0.0"
  }
}
```

```sh
# /etc/hosts
10.13.241.26 dev-cassi.sports.weibo.cn
```









## See Also







