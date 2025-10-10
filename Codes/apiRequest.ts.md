We are using the #atomic-design pattern  for this file. It helps to do atomicity of the code.
So in the following code , we perform easier HTTP request using axios without reinitializing the axios import. 

```typescript
import axios, { type AxiosRequestConfig  } from "axios";

export const apiRequest = {
  get: (url: string, headers?: AxiosRequestConfig["headers"]) =>
    axios.get(url, { headers }),

  post: (url: string, data?: any, headers?: AxiosRequestConfig["headers"]) =>
    axios.post(url, data, { headers }),

  put: (url: string, data?: any, headers?: AxiosRequestConfig["headers"]) =>
    axios.put(url, data, { headers }),

  patch: (url: string, data?: any, headers?: AxiosRequestConfig["headers"]) =>
    axios.patch(url, data, { headers }),

  delete: (url: string, headers?: AxiosRequestConfig["headers"]) =>
    axios.delete(url, { headers }),
};

```


So we can call it as  :

```typescript
apiRequest.method(url,data?,headers?);
```
