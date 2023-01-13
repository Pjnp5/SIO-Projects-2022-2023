# **SIO 2nd Assignment - Bingo**

| NMec   | Email                 | Name             |
| ------ | --------------------- | ---------------- |
| 102491 | raquelparadinha@ua.pt | Raquel Paradinha |
| 103234 | paulojnpinto02@ua.pt  | Paulo Pinto      |
| 103341 | miguelamatos@ua.pt    | Miguel Matos     |
| 104142 | tiagogcarvalho@ua.pt  | Tiago Carvalho   |

</br>
</br>

# Report

## Requirements

* In order to implement support for the Portuguese Citizen Card we used the resources from the Lab class - Smartcards and the PTeID. This way, to run our project it is required that you install the dependencies to create and read virtual smartcards. You can find a tutorial in this link, but keep in mind that it may vary between computers, https://sweet.ua.pt/jpbarraca/course/sio-2223/lab-cc/#virtual-smartcard.
  Some changes we had to make are the following:

  - updated the module

    ```python
    python3-crypto
    ```

    to

    ```python
    python3-cryptography
    ```

  </br>

  - passed the pip3 command to

    ```python
    pip install argparse cryptography
    ```

  </br>

  - installed the pcscd module

    ```bash
    sudo apt update
    sudo apt -y install pcscd
    ```

  </br>

  - installed opensc

    ```bash
    sudo apt update 
    sudo apt install opensc-pkcs11 libpam-pkcs11 pcscd
    ```

    </br>
* FiThen,nter the proje  ct folder (assignment-2---bingo-grupo24), create a virtual environment and run the following command to install the additional requirements to run the project:

  ```bash
  pip install -r requirements.txt
  ```

</br>
</br>

## Running the game

* To run the game you will need a playing area, a caller and at least two players. Each one will be running in its own terminal. You also need to setup the Citizen Card (CC) authentication service.
* To run the CC authentication service, you must set up 2 terminals:

  * The first one hosts the card reading service.

    ```bash
    systemctl stop pcscd
    sudo pcscd -f -d
    ```
  * The second one acts as a virtual card slot. You can only have one card inserted at a time, but after authenticating a user, you must plug in another one by terminating the process and starting a new one with another card.

    ```bash
    ./vicc -t PTEID -v -f <cc_name.json>
    ```
* To run the playing area:

  ```bash
  python3 playingArea/parea.py <port>
  ```
* To run the the caller:

  ```
  python3 caller/caller.py <port> <nickname> <N> <cc_name>
  ```
* To run the player:

  ```bash
  python3 player/player.py <port> <nickname> <cc_name>
  ```

</br>
</br>

## Game Flow

1. Caller and players connect to playing area, providing a valid Portuguese Citizen Card.
2. Caller gives order to start the game.
3. Every player creates a card and commits them to the game.
4. All cards are verified and validated.
5. By connection order, everyone shuffles and encrypts the deck, starting and ending in the caller.
6. After this, everyone posts their keys to decrypt the deck.
7. Everyone computes the winners given resulting decryption of the deck.

</br>
</br>

## User commands

* When all the users and playing area are running and authenticated with their cc, the caller and the players will be able to run commands via user input.

  </br>

  * Caller and Player:

    * **Run at any time**

      ```
      > logs
      ```

      Running this will send an "auditLogRequest" message to the playing area that responds with an "auditLogResponse" that contains all the logs.

      ```
      > users
      ```

      Running this will send an "userListRequest" message to the playing Area that responds with an "userListResponse" that has all users and their information.
    * **After receiving the final deck**

      ```
      > checkwinner
      ```

      This comand only works if all symmetric keys and the final deck have been received. The final deck will be decrypted and the winner/winners will be evaluated, after reaching to a winner the player/caller will send a "resultsSend" message with the winner/winners and decrypted deck to the playing area which will send it to the caller.
      The caller checks if the results are the same as his, if they don't he kicks the player.

  </br>

  * Caller Exclusive

    ```
    > kick <nickname>
    ```

    This command is used to manually kick a player from the game. A "kick" message is sent to the playing area which will process the kick and send that message to all the users that were kicked, ending their connections. The remaining players will receive an updated users list.

    ```
    > start
    ```

    This command starts the game. If everything is in order upon sending this command, the game will start and the results will be computed, according to the game flow.
    After this command, the game itself runs on its own, without needing input from neither caller nor players. Any non-conformity with the expected results is disqualification, being it invalid signatures, bad cards (repeated numbers, to small, etc...), bad deck handling, etc.

</br>
</br>

### Every game, there is a chance someone will handle something incorrectly. This is purely probabilistic, and when it does happen, a message is displayed in the terminal where it happened.

</br>

### Send wrong public key

* When the player is suposed to send is assymetric public key he instead creates a new one from a diffrent private key and sends it.

</br>

### Mess with the deck

* When the player recieves the deck, he will create a new one, encrypting with his symmetric key and sending to the next player.

</br>

### Send invalid card

* When the the game starts, the player creates an card with duplicated numbers and sends it.

</br>

### I'm the winner

* The player instead of checking the winner of the game just decrypts the deck and says he was the winner (if he really is the only winner it's like he did not cheat).

---
