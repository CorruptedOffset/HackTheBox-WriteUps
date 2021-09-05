<h1> 
  Racecar Writeup
  </h1>
  
  Racecar is a challenge in the Pwn section of Hack The Box platform. The challenge is worth 20 points, so this is a rather easy challenge. However, it isn't called a challenge for nothing. This is a great challenge for newcomers to the world of Binary Exploitation. This was the second challenge I have ever attempted on HTB and the first one in the pwn section. As easy as this challenge was, I definitely learned where my weaknesses are and how to improve upon them.\
  \
  First I downloaded the file and decided to run the program just to get a feel for everything: \
  ![Start screen of Racecar program](https://github.com/CorruptedOffset/HackTheBox-WriteUps/blob/main/Challenges/Pwn/racecar_startScreen.png)\
  I continued to move through the program just seeing how it normally operates and noting where and when it asks for user input. Eventually I found myself initiating a race and winning! More importantly though, I saw a clue that caught my attention: 
 ![Flag reference](https://github.com/CorruptedOffset/HackTheBox-WriteUps/blob/main/Challenges/Pwn/racecar_winNoFlag.png)
  \
  Interesting. So the program seems to be reading the flag upon a winning the race. Will it print it if we win and there is a flag file present? 
  Let's create a flag file and see if it prints anything upon victory: \
  ![Creating the flag](https://github.com/CorruptedOffset/HackTheBox-Writeups/blob/main/Challenges/Pwn/racecar_flagCreation.png)
  Now that we have created the flag, let's see if the contents of the flag file get printed. 
  ![Flag Not Printing](https://github.com/CorruptedOffset/HackTheBox-WriteUps/blob/main/Challenges/Pwn/racecar_localwinFlag.png)\
  Hmmmm. So even if we create a flag.txt file in the directory, it still won't print. Let's take a look under the hood and see what we have going on. 
  
  For this challenge, I used Ghidra to conduct my static assembly. For those who don't know, Ghidra is a software reverse engineering tool that was developed by the National Security Agency (NSA) and released as an open-source software. It is really powerful and the free price tag is definitely attactive too. 
  
  So we fire up Ghidra and load in our binary: 
  ![Ghidra start](https://github.com/CorruptedOffset/HackTheBox-WriteUps/blob/main/Challenges/Pwn/racecar_ghidraMain.png)
  So off the bat, we already notice a lot of functions and information floating around. It's easy to get lost in it all. There is a lot of stuff going on, but we can very easily narrow things down. Going back to our first poke around the software, we saw that winning the race led to the program attempting reading the flag.txt file. We also saw that it printed a phrase that explicitly said "flag.txt". So let's search for that string in the program. Ghidra allows string searches by going to Search>Program Text. From there we enter flag.txt and see where it is referenced. 
  
  ![Flag.txt search](https://github.com/CorruptedOffset/HackTheBox-WriteUps/blob/main/Challenges/Pwn/racecar_flagTXTSearch.png)
  
  We can immediately see that the string "flag.txt" is referenced in the car_menu function. So we navigate there to see what is going on there. 
  
  
