## Local verification


### NodeJS
1. Run the lgtm container `docker-compose up`
2. Build this autoinstrumentation `npm install`
2. Modify your service npm start script to look something like
    ```json
    {
        ...
        "scripts": {
            "start": "tsc && node dist/src/server.js",
            "start:otel": "tsc && node --require <path-to-this-repo>/build/workspace/autoinstrumentation.js dist/src/server.js",
            ...
        }        
        ...
    }
    ```
3. Export following env variable (Optional)
    ```bash
    export OTEL_SERVICE_NAME=foo-local
    export OTEL_PROPAGATORS=tracecontext,baggage
    export OTEL_NODE_RESOURCE_DETECTORS=env,host,os,process
    ```
4. Run your service
    ```shell
    npm run start:otel
    ```
5. Trigger request or workflow so your app start sending metrics to Tempo
6. Open http://localhost:3000
    - Login with `admin` and `admin` for username and password
    - Go to `Explore`
    - Select Tempo as datasource (for Trace)
    - Search traceId you found in your log

### JVM
1. Run the lgtm container `docker-compose up`
2. Download OTel agent
    ```shell
    curl -LO https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar    
    ```
3. Export following env variable
    
    **Optional**
    ```bash
    export OTEL_SERVICE_NAME=foo-local
    export OTEL_PROPAGATORS=tracecontext,baggage
    export OTEL_NODE_RESOURCE_DETECTORS=env,host,os,process
    ```

    **Mandatory**
    ```bash
    export JAVA_TOOL_OPTIONS=-javaagent:opentelemetry-javaagent.jar
    ```
4. Run your service
    ```shell
    java -jar build/libs/foo-1.2.3.jar
    ```
5. Trigger request or workflow so your app start sending metrics to Tempo
6. Open http://localhost:3000
    - Login with `admin` and `admin` for username and password
    - Go to `Explore`
    - Select Tempo as datasource (for Trace)
    - Search traceId you found in your log
