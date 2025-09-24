# ChadAI: A chatroom with AI prompter.
We fine tuned Mistral 7B model and integrated it in our very own chat room interface, so the AI agent can prompt you in your style of talking.
<br/> <br/>
A snapshot of the chatroom interface and the AI prompt. You can press Tab key to incorporate prompt in your chats.<br/>
<img width="502" alt="chatroom" src="https://github.com/heet434/chadAI/assets/118350153/b6a7a404-f3cc-4341-a09a-dea49d1e36f4">
<br/><br/>
Here is an example of how our AI model performs, in a simple GUI environment we created for users to directly interact with the model. To incorporate a prompt, press Tab key.

https://github.com/heet434/chadAI/assets/118350153/1b04b5fb-8eaf-4cce-b9f7-aa334c9adae9

Click this link for a complete working demo video of the app: [demo_video](https://youtu.be/pwR5q8MuECg?list=PLLKRQN6ufBc4EmP-kUI0tSSCUgRH8jDMe)


## IMPORTANT

Note that this model was trained on Apple MacBook M1 Pro environment and uses Apple's MLX framework for fast computing using Apple's integrated Metal GPUs inside Apple Silicon. 
To run our program, or to train on your own custom data, you will need an Apple Silicon environment.

## INFORMATION

Our Project uses MLX to fine-tune Hugging Face's Mistral-7B model on chats to replicate a chatting environment that resembles the user. The Mistral-7B model is quantized and then fine-tuned on custom data in local MacBook environment. 
I share with you the fine-tuned fused model, some environments to run the model and also the steps on how to fine-tune the Mistral-7B model on your own chats.

## DEPENDENCIES

Run the following command in your terminal.

```
pip install -r requirements.txt
```

After this, you'll need the Mistral-7B model from Hugging Face. Run the following command in your terminal.

```
python ./AI/convert.py --hf-path mistralai/Mistral-7B-v0.1 --mlx-path ./AI/mlx_model -q
```
This will download the Mistral-7B model from Hugging Face and convert it to a quantized model for faster inference. It will be saved in ./AI/mlx_model folder.

## INSTRUCTIONS

### Using the AI integrated chatroom:


[These are the instructions to run the chatroom locally. Running the chatroom app on multiple devices over a network is out of the scope of this readme.]

After tkinter is installed, run the following command in the terminal;
```
python ./server.py localhost
```
This will start the server for the chatroom. Also an address will be shown in the terminal (eg. for me it is 125.1.0.1), copy this address.
Now go to another terminal in the same folder, and type the command
```
python chatroom_client.py 125.1.0.1
```
Use your own copied address in the command, here I have used mine.
This will start a program which will ask for your name, enter name and a window appears.
Type whatever you want to send in the input box provided. If you wait for 2 seconds, a response will be generated in the AI response box according to what you typed and the previous messages in the chat. Press tab key to use the first word from the response.


### Using the fine-tuned fused model:

To use the fine-tuned fused model in a GUI environment, run the gui.py python file on your local environment. 

```
python gui.py
```

A window like this will open, enter whatever prompt you want to give, and in the generated text box, the AI generated customized text  suggestion will appear. 


If you press tab, the suggestion will be accepted and used in your text. 
You can also keep typing and looking for other suggestions.


You can adjust the confidence level of the model by adjusting the temperature in gui.py.

To run the fused model in terminal, type the following command in terminal

```
python lora.py --model ./AI/mlx_model \
               --adapter-file ./AI/whatsapp.npz \
               --max-tokens 100 \
               --prompt \
               "Heet: Hey guys, "  #(this should be your prompt part)
```             

You get an output similar to: 

```
Loading pretrained model
Total parameters 1244.041M
Trainable parameters 1.704M
Loading datasets
Generating
Heet: Hey guys, have you heard about the cycle thefts happening on campus recently? It's getting crazy, man.
 Onkar: Ohh yeah, it's been going around. Umm, someone even stole my friend's bike last week. It's becoming a real nuisance.
  Prince: Damn, that's messed up. I mean, we're all students here trying to focus on studies, and then we have to worry about our bikes getting
==========
```

### To fine-tune model on your own data:

To export WhatsApp chats:
Go to WhatsApp -> Settings -> Export Chat -> Select group conversation -> Without Media
Unzip the file and save the chats as chat.txt in the folder.

Make sure the dependencies are installed by running the command 

```
pip install -r requirements.txt
```

in terminal.

Run the following command to generate training file in the format for the model:

```
python whatsapp.py --input_file chat.txt --output_file chat.jsonl --test_file ./AI/mydata/test.jsonl --train_file ./AI/mydata/train.jsonl --valid_file data/valid.jsonl
```

Then to train the model, use the command

```
python ./AI/lora.py --model ./AI/mlx_model --train --iters 1000 --data ./AI/mydata --batch-size 2 --adapter-file ./AI/whatsapp.npz
```
Note that I have already uploaded my data and its adapter file as whatsapp.npz. You can use your own data and adapter file. <br/>

For my system, MacBook Pro 14" with M1 Pro chip, it took about 8 hours for complete training (all 1000 iterations).

After training, you are good to go and can use the model as mentioned before.

## CREDITS

The models directory, convert.py, fuse.py, lora.py, models.py, and utils.py are taken from https://github.com/ml-explore/mlx-examples and modified for our use case. Same goes with whatsapp.py, which is taken from https://github.com/gavi/mlx-whatsapp and modified. I thank the authors for their work, and their code has been very helpful in our project. Also, big thanks to Hugging Face for their Mistral-7B model, which we fine-tuned for our project.

