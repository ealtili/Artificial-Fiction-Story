# Artificial-Fiction-Story
AI Powered Story Writing
Artificial intelligence is a hot topic and in this tutorial we will try to use AI to write very own story writing robot. We willtry to train an AI and hopefully find something useful within it's writing. 

These tutorials are to guide through the steps needed to create AI to write artificial fiction. We will use windows computer however if you like you can also do it on mac as well.

Requirements
1) Install Docker
(https://docs.docker.com/docker-for-windows/)

2) Download Model 
In this tutarial we will use an existing model and in the next tutorial we will train a custom model.
The AI is an LSTM Neural Network. Model file stored in a .t7 and name your file with something like project.t7. Place it in a new folder with the same name in your Documents folder. You will need to replace name in all command line arguments with the name of your project.

Download convert_gpu_cpu_checkpoint.lua

3) Start a Docker Container
Start up a Docker Container with all of the packages and code. We will be using a program called torch-rnn written in the programming language Lua by Justin Johnson (based on char-rnn by Andrej Karpathy). Cristian Baldi has created a Docker Image we can download automatically that has the program and all of its dependencies set up ready to go. 

Run the following code into the command prompt and hit return:

docker run -ti --rm --name literai crisbal/torch-rnn:base

It will take a few moments to download everything you need. When it is done the command prompt will come up again indicating that the process finished correctly. Now the command prompt is controlling the inside of your Docker Container.

4) Copy Model into Docker Container
The next step is to copy the model from your computer into the Docker Container. To do this you need to open a new Command Prompt window (go to the start menu, search for cmd, and open it like before). We need a second window as our first window is now controlling the Docker Container.

Paste the following command into the second command prompt window and hit return to copy the folder you made of your model across. If the folder with your model isn't in your Documents folder you will need to modify the path in the code to link to the right place. Remember to replace project with your own project name.

docker cp C:\Users\Admin\Documents\project\ literai:root/torch-rnn/project/


Copy convert_gpu_cpu_checkpoint.lua to docker container



Convert Lua pretrained models to your project directory

Training with GPU but sampling on CPU. Right now the solution is to use the convert_gpu_cpu_checkpoint.lua script to convert your GPU checkpoint to a CPU checkpoint. In near future you will not have to do this explicitly. E.g.:

$ th convert_gpu_cpu_checkpoint.lua cv/lm_lstm_epoch30.00_1.3950.t7
will create a new file cv/lm_lstm_epoch30.00_1.3950.t7_cpu.t7 that you can use with the sample script and with -gpuid -1 for CPU mode.

Happy sampling!

5)Write Text

Now that everything is set up we are finally ready to write some text! For this step we need to go back to the first command prompt where we will copy and paste the command below. Note that when you are working with the command prompt controlling the docker container pasting with Ctrl+V won't work - instead you can right click on the line where you want to paste.

th sample.lua -checkpoint project/project.t7 -length 2000 -temperature 0.7 -gpu -1

After a couple of minutes the computer should print out 2000 characters of text! Your AI has just written entirely novel literature! You can copy this text out of the comand prompt into a text editor (like Word or Google Docs).

To create more text, simply paste the command again (or hit the up arrow in the command prompt to auto-populate it). You'll notice in the command is the phrase -length 2000 You can modify this number to be bigger or smaller to see more or less text. In practice you can sometimes get better results by generating lots of shorter snippets rather than a really long one. There are other parameters you can tweak here to get better results. The tips and tricks tutorial goes over these in more detail.

When you are done generating text it is important to enter one last command into the first command prompt window:

exit

This will close down the Docker Container and will delete everything on it. Importantly, this will also free up the name "literai" which we gave to the container in step 3. If you forget to exit and then come back to write more fiction in the future the code we have will cause an error, since the name "literai" will still be taken. You can solve this by using a different name (eg literai2) in all of the commands, but for simplicity remember to type exit!

