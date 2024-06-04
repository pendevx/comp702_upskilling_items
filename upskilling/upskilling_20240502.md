# CloudFlare Upskilling

Main areas of upskilling for CloudFlare is:
1. Wrangler - what is it?
2. Cloudflare Workers
3. Service Bindings
4. Cloudflare Workers integrations with Cloudflare R2

## Wrangler

Wrangler is cloudflare's development tool. It encapsulates other tools and processes such as `miniflare` and `wranglerd`, which then also contain functionality such as creating development environments for cloudflare workers, or running local integrations for other cloudflare services, or simply hosting a development server with hot reload to help speed up development processes whilst unlocking the ability for developers to perform local development.

There is also a `wrangler.toml` file, which is the configuration file for Cloudflare to use when you are declaring secrets amd other configurations, such as integrations, service bindings, and environment variables.

## Cloudflare Workers

Cloudflare Workers are a serverless Functions as a Service (FaaS) service, allowing to run JavaScript/TypeScript code in a serverless environment on Cloudflare's global network.

When a worker has been deployed, it will be publicly accessible by a single public endpoint, which will invoke the function when traffic hits that endpoint.

Cloudflare Workers uses a runtime environment created by Cloudflare, which is separate from Nodejs. The entry point to workers are an async `fetch()` method, which must be contained within the default export from the specified entry file of the worker.

## Service Bindings

In order for two Cloudflare Workers to interact with each other, it is necessary that Service Bindings are set up. 

There are two types of service bindings:
1. RPC, allowing communication between workers via a function the developer defines
2. HTTP, allowing communication between workers via the entry point `fetch()` method on the invoked worker

The service bindings must be set up in the `wrangler.toml` configuration of the worker which will invoke the other worker. It will look like the following:

```toml
services = [
    { binding = "<BINDING_NAME>", service = "<WORKER_NAME>" }
]
```

### RPC endpoints

An example of a worker which exposes an RPC endpoint:

```js
import { WorkerEntrypoint } from "cloudflare:workers";

export class InvokingWorker extends WorkerEntrypoint {
  async add(a, b) { return a + b; }
}
```

This code can be called from another worker as such:

```toml
services = [
  { binding = "WORKER_B", service = "worker_b" }
]
```

```js
const result = await env.WORKER_B.add(1, 2);
```

### HTTP bindings

In HTTP bindings, the invoking worker just needs to call the `fetch()` entry point to the other worker and supply a `Request` object, then expect a `Response` object in return:

```toml
services = [
  { binding = "WORKER_B", service = "worker_b" }
]
```

```js
const result = await env.WORKER_B.fetch(new Request(...));
```

## Cloudflare Workers integrations with Cloudflare R2

Cloudflare allows bindings between workers and many other Cloudflare services. Since the project we are working on uses R2, I decided to study R2 integrations specifically.

In order for a worker to integrate with the R2 bucket, the following configuration must be present in the `wrangler.toml` file:

```
[[r2_buckets]]
binding = 'MY_BUCKET' # <~ valid JavaScript variable name
bucket_name = '<YOUR_BUCKET_NAME>'
```

Once the configuration is complete, the worker may interact with the R2 bucket via the field populated on the environment variable:

```js
// examples
const object = await env.MY_BUCKET.get(key);
await env.MY_BUCKET.put(key, request.body);
await env.MY_BUCKET.delete(key);
```

# Upskilling Artefacts

For this upskilling, I created a simple application which serves static files from an R2 bucket. The application uses Server-side Rendering (SSR), using the `hono` framework for Cloudflare Workers. This worker contains a service binding to the R2 storage, in order to read the file names and populate them into the HTML dynamically on the server.

There is also another second worker, which contains a binding to the R2 storage, in order to serve the static files.

This upskilling artefact taught me how to use Cloudflare workers, as well as allowing me to put my knowledge of service bindings and other Cloudflare service integrations to use. It also allowed me to begin familiarizing with JSX, the language used in React, which is another piece of technology used in our RND project.

The project artefacts are placed in the `hono/` folder, and the `music-worker/` folder.

This project took me 6 hours in total:
- 2 hours on the 3rd of May
- 4 hours on the 4th of May
