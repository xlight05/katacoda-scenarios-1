FaaS is a definition language that allows developers to declare serverless compositions without being locked in to a vendor. Developers can compile the defined composition during compile time to any cloud provider. Currently, AWS and Knative are supported as the target cloud providers.

Click on the following command to download FaaS compiler and add it to bin file to access globally.
`wget https://github.com/xlight05/codefass/releases/download/0.1-pre/fass && chmod +x fass && cp fass /bin`{{execute}}


Great! You now have a running Faas Compiler running. 

Let’s download a sample serverless project that has four components.
`wget https://github.com/xlight05/codefass/releases/download/0.1-pre/fyp-demo.zip && unzip fyp-demo && cd fyp-demo && touch demo.fass`{{execute}}

Now lets create the Definition file in order to orchastrate the serverless functions.

Open the definition file.
`fyp-demo/demo.fass`{{open}}


Copy the following code to the file.
<pre class="file" data-filename="fyp-demo/demo.fass" data-target="prepend">
function seq1 = {
    name:"sequenceOne"
    handler:"seq1/app.js"
    language: "nodejs"
};
function seq2 = {
    name:"sequenceTwo"
    handler:"seq2/app.js"
    language: "nodejs"
};

function par1 = {
    name:"parallelOne"
    handler:"par1/app.js"
    language: "nodejs"
};
function par2 = {
    name:"parallelTwo"
    handler:"par2/app.js"
    language: "nodejs"
};

orchestrate ($type) {
    if ($type=="seq") {
        sequence seq = [seq1 seq2];
    } else {
        parallel par = [par1 par2];
    }
}

</pre>

Finally, its time to generate the clcoud provider native artifacts.

Generate AWS Artifacts
`fass build aws`{{execute}}

Generate Knative Artifacts
`fass build knative`{{execute}}
