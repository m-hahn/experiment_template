# Instructions

In this tutorial, you will post an experiment to the Mechanical Turk sandbox. This involves three steps:
1. Creating an experiment and uploading it to your Stanford webspace. For this tutorial, we provide a dummy experiment that you can use.
2. Creating an Amazon Web Services Account and installing the MTurk Command Line Tools.
3. Posting your experiment. You'll be using a Python script that interfaces nicely with the MTurk Command Line Tools.


## Put the Experiment on your Webspace

1. Open the command line and move to the place where you want to put the experiment.

2. Execute `git clone https://github.com/m-hahn/experiment_template.git`

If you have not yet installed git, an alternative is to go to https://github.com/m-hahn/experiment_template, click on `Clone or download`, then `Download ZIP`. After downloading, unpack the archive at the intended place.

3. Open a new tab in your browser, and paste the following path into the address bar: `PATH/experiment_template/template/template.html`, where `PATH` is the path where you put the experiment template in the previous step. (You can use `pwd` to display the current path in the command line.) Hit Enter to open this page.

4. You should now see the welcome page of a mock experiment. You can click through it to see what it looks like. The second and third slide should contain the Consent Form from https://linguistics.stanford.edu/resources/experimental-protocol. If you are already familiar with HTML and Javascript, you can adapt the template for your needs. In any case, for this workshop, we'll pretend that this is the experiment that you want to run.
 
5. In order for participants to see you experiments on the web, you will now upload it to your Stanford webspace.

6. Open a new command line tab. Type `ssh YOUR_SUNET_ID@cardinal` and hit enter. Go through the authentication procedure.

7. Execute `ls WWW`. Unles you have already put things in your webspace, nothing should show up. If an error message appears, execute `mkdir WWW`.

8. Going back to the previos tab, execute `scp -r experiment_template YOUR_SUNET_ID@cardinal:~/WWW/`

9. Once this has finished, open `http://stanford.edu/~YOUR_SUNET_ID/experiment_template/template/template.html` in your browser. The experiment should show up. Now your experiment is on the web!


## Install Amazon MTurk Command Line Tools

1.    Create an Amazon Web Services (AWS) account at https://aws-portal.amazon.com/gp/aws/developer/registration/index.html.
2.    Sign up for an Amazon Mechanical Turk Requester account at the https://requester.mturk.com/.
2.    Now you will link your AWS Account and your MTurk Requester Account. Open https://requester.mturk.com/developer, and click `Link your AWS Account`. Then sign-in with your AWS Root user email address and password.
3.    Download and install Java Runtime Environment (JRE) from: http://www.oracle.com/technetwork/java/javase/downloads/index.html.
4.    Download and unzip Amazon Mechanical Turk Command Line Tools from https://requester.mturk.com/developer/tools/clt
5. Go to https://aws.amazon.com/ and log into your AWS account.
Click on your name and then `My Security Credentials`.
Click on `Get started wih IAM Users`.
Click on `Add User`.
5.    Follow our instructions (see email) to generate an MTurk access key.
6.    At the place where you have unzipped the Command Line Tools, there should be a folder `aws-mturk-clt-...`.
7.    Inside this folder, there is a `/bin` directory, inside which there is a file called `mturk.properties`. Open this file in a text editor.
8.  There should be lines 
```
access_key=[insert your access key here]
secret_key=[insert your secret key here]
```
Paste the keys from the previous step in the place of the placeholders.
9. Save the file. Important: This file now contains your secret key, and anyone having this file can post HITs in your name. It might be reasonable to remove the keys once you're done and generate new keys when you do your next experiment.


## Post your Experiment to MTurk

In this section, you will post the experiment to the MTurk Sandbox.

1. To post experiments, you will be using a Python script. If you have not installed Python, you will first need to install it. You can download Python from https://www.python.org/downloads/. The script will definitely work with Python 2.7, so we recommend downloading this.

1. In the command line, navigate to the place where you have downloaded the experiment. Execute
 
```
git clone https://github.com/feste/Submiterator.git
```

Again, if you have not installed git, you can go to https://github.com/feste/Submiterator, and click on `Clone or download`. Extract the downloaded zip file.

2. In either case, you will now have a directory called `Submiterator` at the place where you downloaded the script.

2. Open a text editor and paste the following:

```
    {
    "rewriteProperties":"yes",
    "liveHIT":"no",
    "title":"a title to show to turkers",
    "description":"a description to show to turkers",
    "experimentURL":"https://stanford.edu/~YOUR_SUNET_ID/experiment_template/template/template.html",
    "keywords":"language research stanford fun cognitive science university explanations",
    "USonly?":"yes",
    "minPercentPreviousHITsApproved":"95",
    "frameheight":"650",
    "reward":"0.00",
    "numberofassignments":"1",
    "assignmentduration":"1800",
    "hitlifetime":"2592000",
    "autoapprovaldelay":"60000",
    "conditions":"cond"
    }
```

In the sixth line, replace `YOUR_SUNET_ID` with your SUNET ID.

Save this file inside the `Submiterator` directory that you have, giving it a name ending in `.config`, such as `my-first-experiment.config`.

3. Run the following commands:
```
export MTURK_CMD_HOME=PATH_WHERE_YOU_HAVE_UNZIPPED_AWS_CLT/aws-mturk-clt-1.3.4
export JAVA_HOME=PATH_OF_YOUR_JAVA_RUNTIME_ENVIRONMENT
```
The PATH_OF_YOUR_JAVA_RUNTIME_ENVIRONMENT might be `/usr`.

The first one tells the Python script where to look for the AWS Command Line Tools, the second one makes sure the Java programs contained in it can be executed.

3. Open the command line and navigate into the `Submiterator` directory. Run the following command
```
    python submiterator.py posthit my-first-experiment
```
This will return a message giving the URL to the experiment on MTurk, and generate a bunch of files.
The script stores the HIT's ID in the `my-first-experiment.success` file, so make sure not to delete this one before you have downloaded the results. If you're running an expensive experiment, it might make sense to backup this file.

The name after `posthit` must match the name of the `.config` file that you created previously.

4. To get the results:
```
    python submiterator.py getresults my-first-experiment
```

5. To get a list of results by trials, run
```
    python submiterator.py reformat my-first-experiment
```
which will generate a set of `.tsv` files giving results by trials and subjects.


# Beyond this Tutorial

Here are some notes for running experiments beyond this tutorial.
Of course, you're welcome to come to us with any questions about creating and running your own experiments.

## The .config File

Consider the `...config` file that you created in the `Submiterator-master` folder.
For every HIT that you post, you will need to create a new `...config` file.
Change the value of `liveHIT` to `yes` once you're ready to post your HIT for workers to do.
Edit values for title and description for your experiment.
`reward` indicates the payment per participant. Note that this is only the amount that a participant gets, to this Amazon adds a fee.
`numberofassignments` indicates how many participants you want for the HIT. The fee will depend on the number of participants, so you might want to create a series of smaller HITs.
`assignmentduration` indicates how many seconds after a worker has accepted the HIT it will expire. Give ample time so that workers are not rushed.

```
    {
    "rewriteProperties":"yes",
    "liveHIT":"no",
    "title":"a title to show to turkers",
    "description":"a description to show to turkers",
    "experimentURL":"https://stanford.edu/~YOUR_SUNET_ID/experiment_template/template/template.html",
    "keywords":"language research stanford fun cognitive science university explanations",
    "USonly?":"yes",
    "minPercentPreviousHITsApproved":"95",
    "frameheight":"650",
    "reward":"0.00",
    "numberofassignments":"1",
    "assignmentduration":"1800",
    "hitlifetime":"2592000",
    "autoapprovaldelay":"60000",
    "conditions":"cond"
    }
```

## Multiple Participation

If you have a series of HITs and want to ensure that every worker participates only in one HIT, you can use a code snippet provided at https://uniqueturker.myleott.com/.

## Instructions for Building off this Template

Inside the _shared are some helper js files, some libraries we call. Some of these are completely external, like jquery and raphael, some of them are written by members of the lab, like mmturkey and utils.

Inside your javascript file for your own experiment, e.g. template.js, you'll write up the structure that the slides have. In template.js, the line

    exp.structure=["i0", "instructions", "familiarization", "one_slider", "multi_slider", 'subj_info', 'thanks'];

defines the logical flow of the experiment. Always start with i0 (the consent slide) and move on from there. The functions that use this structure to navigate through the experiment are in stream-V2.js and exp-V2.js.  Each slide has several parts:

1. the code in the `start` function is run at the beginning of that block
2. if there is a `present` data structure (which can be a list of objects of any kind), each of these will be passed in, one at a time, to `present_handle`, for each trial in that block
3. `present_handle` has the parameter `stim` from the `present` data structure, and this code is run at the beginning of every trial.
4. the code in `button` is called directly from the html where the button is defined. `_s` always refers to the current slide, so you can reference any function you define as a property of the slide by using the `_s.FUNCTION_NAME()` syntax.
2. the `end` function is run at the end of the block
6. you can define any other functions you want inside the slide and call them either with `this.FUNCTION_NAME` or `_s.FUNCTION_NAME()`.

We've provided a progress bar, which required that you calculate the total number of questions in your experiment (just run `exp.nQs = utils.get_exp_length();` inside the `init` function, at some point after `exp.structure` is defined). Turkers like this a lot.
