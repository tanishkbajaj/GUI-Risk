#Risk

This project offers a platform for which multiple users can write bots to play Risk.

The rules of Risk can be found here: http://www.hasbro.com/common/instruct/risk.pdf
Note that this version of Risk does not include missions.

##Architecture
####Game Master and Players
Entry Point for game is in src/Master/GameMaster.java

There exists a Player interface, defining all required functionality of an automated player, which any bot must implement.

The platform follows a Request-Response architecture, wherein the Game Master repeatedly asks each player for their decision at each decision point, and the Player who is prompted will respond with their choice. Certain responses are optional, such as attacking and fortifying. Others are always required, such as reinforcing, defending, and advancing armies into a conquered territory. Still others are only required when certain conditions are met, such as turning in cards.

When an invalid response is received from a Player, the Game Master will attempt a small number of retries, but will ultimately eliminate any Player who fails to respond as per the rules. The reason for these retries is to allow for the maximum flexibility of the Player classes in how they operate, which can make the game more interesting.

With each request, the Game Master provides the Player with all relevant information concerning the curent state of the game, to allow the Player to make as informed or naive decisions as they would like.

####Domain Model
The Risk architecture is built around 3 primary data structures: Continent, Country, and Card. A continent contains a name attribute and multiple member countries. A country contains name, owner, and numArmies attributes, as well as a list of neighboring countries. The entire map can be traversed through the graph formed by these two geographical classes. Finally, a card has two attributes, a type and a country (unless it is a wild card).

A class RiskMap builds and holds the map information and state, and a copy of it is passed to each Player along with each request.

Additionally, there is a specialized Response class for each type of response. Each of these classes contains getter and setter functionality for its attributes and also a method isValidResponse() which will return true if the response is valid and false otherwise.

####Other Comments
All relevant constants are stored in the RiskConstants class.
There are some handy static methods in the RiskUtils class that may aid in completing some common tasks within the Player implementation.
There is an example Player class, DefaultPlayer, which uses simple logic and provides a minimalistic working example which can be extended, if desired.
After each event in the game, a line is written out to LOG.txt, which contains a synopsis of the game.
If you write a good helper method (ex: a method that returns a list of boundary countries for a given player), make it as robust as possible, and add it into RiskUtils for everyone to use! This way, different Player implementations can be separated by the logic they use, and not simply by who felt like writing the most code.

##Log Player (New!)
Entry Point for LogPlayer is in src/LogPlayer/LogPlayer.java

The new LogPlayer class allows developers to replay the events of the most recently logged game in a graphical application. This allows for much more efficient and intuitive refinement of your bot. Sure, the UI isn't going to win any awards, but it was thrown together in a weekend (ok, fine, a 3-day weekend).

One important note, this app is built on top of JavaFX, which comes as part of the JDK, so it should simply be a matter of adding that library to your project's classpath, and you'll be good to go!

Feel free to clone the repo, write and test your own Players, and send them in (Pull Requests welcome!) to be pitted against others! You can always look at the other Player implementations, but it's highly recommended that you implement your own ideas first!
