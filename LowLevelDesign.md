1. Twitter
```
 #include <unordered_map>
#include <deque>
#include <vector>
#include <algorithm>
using namespace std;

class User {
public:
    int id;
    unordered_set<int> following;
    unordered_set<int> followers;
    vector<Tweet> tweets;

    User(int userId) : id(userId) {}

    void postTweet(int tweetId, string message) {
        tweets.push_back(Tweet(tweetId, id, message));
    }

    void follow(int userId) {
        following.insert(userId);
    }

    void unfollow(int userId) {
        following.erase(userId);
    }
};

class Tweet {
public:
    int id;
    int userId;
    string message;
    long timestamp;

    Tweet(int tweetId, int userId, string msg) 
        : id(tweetId), userId(userId), message(msg), timestamp(time(0)) {}
};

class TwitterService {
private:
    unordered_map<int, User> users;  // Map of user IDs to User objects
    unordered_map<int, vector<Tweet>> tweets;  // List of all tweets (for simplicity)

public:
    TwitterService() {}

    // Create a new user
    void createUser(int userId) {
        if (users.find(userId) == users.end()) {
            users[userId] = User(userId);  // Store User object directly
        }
    }

    // Post a tweet
    void postTweet(int userId, string message) {
        // Get the user, create tweet and add it to their timeline
        if (users.find(userId) != users.end()) {
            User& user = users[userId];  // Reference to User object
            int tweetId = tweets[userId].size() + 1; // Simple increment for tweet ID
            user.postTweet(tweetId, message);
            tweets[userId].push_back(Tweet(tweetId, userId, message));
        }
    }

    // Follow another user
    void follow(int followerId, int followeeId) {
        if (users.find(followerId) != users.end() && users.find(followeeId) != users.end()) {
            users[followerId].follow(followeeId);
            users[followeeId].followers.insert(followerId);
        }
    }

    // Unfollow a user
    void unfollow(int followerId, int followeeId) {
        if (users.find(followerId) != users.end() && users.find(followeeId) != users.end()) {
            users[followerId].unfollow(followeeId);
            users[followeeId].followers.erase(followerId);
        }
    }

    // Get a user's timeline (tweets from themselves and the users they follow)
    vector<Tweet> getTimeline(int userId) {
        vector<Tweet> timeline;
        if (users.find(userId) != users.end()) {
            User& user = users[userId];  // Reference to User object
            // Add the user's own tweets
            for (const Tweet& tweet : user.tweets) {
                timeline.push_back(tweet);
            }
            // Add tweets from followed users
            for (int followeeId : user.following) {
                for (const Tweet& tweet : tweets[followeeId]) {
                    timeline.push_back(tweet);
                }
            }
        }
        // Sort by timestamp (most recent first)
        sort(timeline.begin(), timeline.end(), [](Tweet a, Tweet b) {
            return a.timestamp > b.timestamp;
        });
        return timeline;
    }

    // Get the news feed (latest tweets from people the user follows)
    vector<Tweet> getNewsFeed(int userId) {
        vector<Tweet> feed;
        if (users.find(userId) != users.end()) {
            User& user = users[userId];  // Reference to User object
            // Collect the tweets from followed users
            for (int followeeId : user.following) {
                for (const Tweet& tweet : tweets[followeeId]) {
                    feed.push_back(tweet);
                }
            }
        }
        // Sort by timestamp (most recent first)
        sort(feed.begin(), feed.end(), [](Tweet a, Tweet b) {
            return a.timestamp > b.timestamp;
        });
        return feed;
    }
};

```
2. Snake & ladder
```     
         class Square {
public:
    int number;  // Position on the board
    bool hasSnake;
    bool hasLadder;
    int snakeTailPosition;  // If there's a snake, this stores the tail position
    int ladderTopPosition;  // If there's a ladder, this stores the top position

    Square(int number) : number(number), hasSnake(false), hasLadder(false), snakeTailPosition(-1), ladderTopPosition(-1) {}
};

class Board {
private:
    int size;  // Number of squares
    vector<Square> squares;

public:
    Board(int size) : size(size) {
        // Initialize squares
        for (int i = 1; i <= size; ++i) {
            squares.push_back(Square(i));
        }
    }

    void addSnake(int mouth, int tail) {
        squares[mouth - 1].hasSnake = true;
        squares[mouth - 1].snakeTailPosition = tail;
    }

    void addLadder(int base, int top) {
        squares[base - 1].hasLadder = true;
        squares[base - 1].ladderTopPosition = top;
    }

    Square& getSquare(int position) {
        return squares[position - 1];
    }

    int getSize() {
        return size;
    }
};
class Dice {
private:
    int sides;

public:
    Dice(int sides) : sides(sides) {
        if (sides < 6 || sides > 18) throw invalid_argument("Dice sides must be between 6 and 18.");
    }

    int roll() {
        return rand() % sides + 1;  // Random number between 1 and sides
    }
};
class Player {
private:
    int id;
    string name;
    int position;

public:
    Player(int id, const string& name) : id(id), name(name), position(1) {}

    void move(int steps) {
        position += steps;
    }

    int getPosition() const {
        return position;
    }

    string getName() const {
        return name;
    }

    void setPosition(int newPosition) {
        position = newPosition;
    }
};
class Game {
private:
    Board board;
    Dice dice;
    vector<Player> players;
    int currentPlayerIndex;

public:
    Game(int boardSize, int diceSides, vector<string> playerNames)
        : board(boardSize), dice(diceSides), currentPlayerIndex(0) {
        
        // Create players
        for (int i = 0; i < playerNames.size(); ++i) {
            players.push_back(Player(i + 1, playerNames[i]));
        }
    }

    void playTurn() {
        Player& currentPlayer = players[currentPlayerIndex];
        int rollResult = dice.roll();
        cout << currentPlayer.getName() << " rolled a " << rollResult << endl;
        currentPlayer.move(rollResult);

        // Check for snake or ladder
        Square& square = board.getSquare(currentPlayer.getPosition());
        if (square.hasSnake) {
            cout << currentPlayer.getName() << " hit a snake! Moving to " << square.snakeTailPosition << endl;
            currentPlayer.setPosition(square.snakeTailPosition);
        } else if (square.hasLadder) {
            cout << currentPlayer.getName() << " climbed a ladder! Moving to " << square.ladderTopPosition << endl;
            currentPlayer.setPosition(square.ladderTopPosition);
        }

        // Check if the player has won
        if (currentPlayer.getPosition() >= board.getSize()) {
            cout << currentPlayer.getName() << " wins!" << endl;
            return;
        }

        // Move to the next player
        currentPlayerIndex = (currentPlayerIndex + 1) % players.size();
    }

    void startGame() {
        while (true) {
            playTurn();
        }
    }
};

```
