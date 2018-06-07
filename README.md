# Node.RPG
Node for RPG use the same development 
Node.RPG is a HTTP server you can bind into your ILE RPG projects, to give you a easy deploy mechanism, that fits into DevOps and microservices alike environments.

The self contained server and application makes it so much easier to develop web application. Simply compile and submit. Yes - You don't need GCI, Apache, nginx or IceBreak - simply compile and submit.

The design paradigm is the same as found in Node.JS hence the name. Where Node.JS uses JavaScript, Node.RPG aims for any ILE language where RPG are the most popular.

Except for initialization, It only requires two lines of code:
```
 node_listen ( config : servletCallback); 
 node_write ( request : response);
```

The `node_liste` are listening on the TCP/IP port and interface you define in the config structure. For each http request it will call you "servlet" which is a callback procedure that takes a request and a response parameter
   
![](image.png)


The idea is that you deploy your (open source of cause) RPG packages at NPM so the RPG community can benefit from each others work. The NPM echosystem is the same for Node.JS and Node.RPG    


Example: 
```
        // -----------------------------------------------------------------------------
        // This example runs a simple servlet using Node.RPG
        // Note: It requires your RPG code to be reentrant and compiled
        // for multithreading. Each client request is handled by a seperate thread.
        // Start it:
        // SBMJOB CMD(CALL PGM(DEMO01)) JOB(NODERPG) JOBQ(QSYSNOMAX) ALWMLTTHD(*YES)        
        // -----------------------------------------------------------------------------     
        ctl-opt copyright('Sitemule.com  (C), 2018');
        ctl-opt decEdit('0,') datEdit(*YMD.) main(main);
        ctl-opt debug(*yes) bndDir('NODERPG');
        ctl-opt thread(*CONCURRENT);
        /include noderpg.inc
        // -----------------------------------------------------------------------------
        // Main
        // -----------------------------------------------------------------------------     
        dcl-proc main;

            dcl-ds config likeds(configDS);

            config.port = 44999;
            config.host = '*ANY';

            node_listen (config : %paddr(myservlet));

        end-proc;
        // -----------------------------------------------------------------------------
        // Servlet call back implementation
        // -----------------------------------------------------------------------------     
        dcl-proc myservlet;

            dcl-pi *n;
                request  likeds(REQUESTDS);
                response likeds(RESPONSEDS);
            end-pi;
  
            node_Write(response:'Hello world');

        end-proc;
```

This first commit we have only implemented the `node_listen` and `node_write`, so there is not much use for real world application, however - we at Sitemule are striving to move the core of the IceBreak server into the Node.RPG project over the next couple of months. So stay tuned.
 
