# Using magic DNS

Tailscale funnel 80 : It is used to create a URL for port 80

and before it we should run the server …..

projects don’t allow the proxy so we need do edit the **`vite.config.js`** (or **`vite.config.ts`**    

```jsx
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

 // or your specific plugin// [https://vitejs.dev/config/](https://vitejs.dev/config/)
 
export default defineConfig({
plugins: [react()],
// Add this 'server' block
server: {
allowedHosts: ['[manoj-retr0.tail8a4785.ts.net](http://manoj-retr0.tail8a4785.ts.net/)'],
},
})
```