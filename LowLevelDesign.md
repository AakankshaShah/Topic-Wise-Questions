Link : https://gitlab.com/shrayansh8/interviewcodingpractise/-/tree/main/src/LowLevelDesign
![image](https://github.com/user-attachments/assets/7ef6e3e6-5c7d-4ec1-bc06-02b3e7792e26)
https://github.com/aakash003/LowLevelDesigns/tree/main



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
3. Distributed Logger system
```
  #include <iostream>
#include <vector>
#include <string>
#include <unordered_map>
#include <queue>
#include <ctime>

using namespace std;

class Log {
public:
    string timestamp;
    string service;
    string level;
    string message;
    string correlationId;

    Log(string service, string level, string message, string correlationId) {
        time_t now = time(0);
        timestamp = ctime(&now);
        this->service = service;
        this->level = level;
        this->message = message;
        this->correlationId = correlationId;
    }

    string toString() {
        return timestamp + " | " + service + " | " + level + " | " + message + " | CorrelationID: " + correlationId;
    }
};

class LogStorage {
public:
    unordered_map<string, vector<Log>> logs;  // key = service name, value = list of logs for that service

    void storeLog(Log log) {
        logs[log.service].push_back(log);
    }

    vector<Log> getLogsByService(const string& service) {
        return logs[service];
    }
};

class LogCollector {
private:
    LogStorage* logStorage;
public:
    LogCollector(LogStorage* storage) : logStorage(storage) {}

    void collectLog(Log log) {
        logStorage->storeLog(log);
    }
};
class LogAggregator {
private:
    LogStorage* logStorage;

public:
    LogAggregator(LogStorage* storage) : logStorage(storage) {}

    vector<Log> aggregateLogs(const string& service) {
        return logStorage->getLogsByService(service);
    }

    vector<Log> searchLogsByLevel(const string& level) {
        vector<Log> filteredLogs;
        for (auto& entry : logStorage->logs) {
            for (auto& log : entry.second) {
                if (log.level == level) {
                    filteredLogs.push_back(log);
                }
            }
        }
        return filteredLogs;
    }
};
class LogProducer {
private:
    LogCollector* logCollector;

public:
    LogProducer(LogCollector* collector) : logCollector(collector) {}

    void produceLog(const string& service, const string& level, const string& message, const string& correlationId) {
        Log log(service, level, message, correlationId);
        logCollector->collectLog(log);
    }
};
int main() {
    // Create a log storage system and a collector
    LogStorage logStorage;
    LogCollector logCollector(&logStorage);
    LogAggregator logAggregator(&logStorage);

    // Create some log producers (simulating different services)
    LogProducer producer1(&logCollector);
    LogProducer producer2(&logCollector);

    // Simulating logs being produced by services
    producer1.produceLog("payment-service", "ERROR", "Failed to process payment", "123");
    producer1.produceLog("payment-service", "INFO", "Payment processed successfully", "124");
    producer2.produceLog("user-service", "INFO", "User registered", "125");
    producer2.produceLog("payment-service", "ERROR", "Payment gateway timeout", "126");

    // Aggregate logs for the payment-service
    vector<Log> paymentLogs = logAggregator.aggregateLogs("payment-service");
    cout << "Payment Service Logs: \n";
    for (const Log& log : paymentLogs) {
        cout << log.toString() << endl;
    }

    // Search for ERROR level logs across all services
    vector<Log> errorLogs = logAggregator.searchLogsByLevel("ERROR");
    cout << "\nERROR Logs: \n";
    for (const Log& log : errorLogs) {
        cout << log.toString() << endl;
    }

    return 0;
}

```
4. Library Management System
```
     class Book {
    private int id;
    private String title;
    private String author;
    private String genre;
    private boolean isAvailable;

    public Book(int id, String title, String author, String genre) {
        this.id = id;
        this.title = title;
        this.author = author;
        this.genre = genre;
        this.isAvailable = true;  // Initially, all books are available
    }

    // Getters and Setters

    public boolean isAvailable() {
        return isAvailable;
    }

    public void setAvailable(boolean available) {
        isAvailable = available;
    }

    public int getId() {
        return id;
    }

    // Other Getters and Setters
}
import java.util.List;

class User {
    private int id;
    private String name;
    private List<Book> issuedBooks;

    public User(int id, String name) {
        this.id = id;
        this.name = name;
        this.issuedBooks = new ArrayList<>();
    }

    // Methods to add and remove books from issuedBooks list
    public void issueBook(Book book) {
        issuedBooks.add(book);
    }

    public void returnBook(Book book) {
        issuedBooks.remove(book);
    }

    public boolean hasIssuedBook(Book book) {
        return issuedBooks.contains(book);
    }

    // Getters and Setters
}

import java.util.Date;

class Transaction {
    private Book book;
    private User user;
    private Date issueDate;
    private Date returnDate;

    public Transaction(Book book, User user) {
        this.book = book;
        this.user = user;
        this.issueDate = new Date();
    }

    // Getters and Setters
    public void returnBook() {
        this.returnDate = new Date();
    }

    public boolean isReturned() {
        return returnDate != null;
    }
}
import java.util.*;

class Library {
    private Map<Integer, Book> bookCatalog;
    private Map<Integer, User> users;
    private List<Transaction> transactions;

    public Library() {
        bookCatalog = new HashMap<>();
        users = new HashMap<>();
        transactions = new ArrayList<>();
    }

    public void addBook(Book book) {
        bookCatalog.put(book.getId(), book);
    }

    public void registerUser(User user) {
        users.put(user.getId(), user);
    }

    public boolean issueBook(int userId, int bookId) {
        User user = users.get(userId);
        Book book = bookCatalog.get(bookId);

        if (user != null && book != null && book.isAvailable()) {
            book.setAvailable(false);
            user.issueBook(book);
            transactions.add(new Transaction(book, user));
            return true;
        }
        return false;
    }

    public boolean returnBook(int userId, int bookId) {
        User user = users.get(userId);
        Book book = bookCatalog.get(bookId);

        if (user != null && book != null && user.hasIssuedBook(book)) {
            book.setAvailable(true);
            user.returnBook(book);
            // Mark transaction as returned
            for (Transaction t : transactions) {
                if (t.getBook().equals(book) && t.getUser().equals(user) && !t.isReturned()) {
                    t.returnBook();
                    break;
                }
            }
            return true;
        }
        return false;
    }

    public Book searchBookByTitle(String title) {
        for (Book book : bookCatalog.values()) {
            if (book.getTitle().equalsIgnoreCase(title)) {
                return book;
            }
        }
        return null;
    }

    public List<Book> getAvailableBooks() {
        List<Book> availableBooks = new ArrayList<>();
        for (Book book : bookCatalog.values()) {
            if (book.isAvailable()) {
                availableBooks.add(book);
            }
        }
        return availableBooks;
    }

    public List<User> getTopUsersWithMostBooks() {
        List<User> sortedUsers = new ArrayList<>(users.values());
        sortedUsers.sort((u1, u2) -> Integer.compare(u2.getIssuedBooks().size(), u1.getIssuedBooks().size()));
        return sortedUsers;
    }
}

```
5. Retail point of sale
```
    class Denomination {
    private double value;   // e.g., 0.25 for 25 cents
    private String name;    // e.g., "25 Cent Coin"

    public Denomination(double value, String name) {
        this.value = value;
        this.name = name;
    }

    public double getValue() { return value; }
    public String getName() { return name; }
}
lass BalanceCalculator {
    // Predefined list of USD denominations, sorted from highest to lowest
    private List<Denomination> denominations = List.of(
        new Denomination(100, "100 Dollar Bill"),
        new Denomination(50, "50 Dollar Bill"),
        new Denomination(20, "20 Dollar Bill"),
        new Denomination(10, "10 Dollar Bill"),
        new Denomination(5, "5 Dollar Bill"),
        new Denomination(1, "1 Dollar Bill"),
        new Denomination(0.25, "25 Cent Coin"),
        new Denomination(0.10, "10 Cent Coin"),
        new Denomination(0.05, "5 Cent Coin"),
        new Denomination(0.01, "1 Cent Coin")
    );

    public Map<Denomination, Integer> calculateBalance(double balance) {
        Map<Denomination, Integer> balanceDistribution = new LinkedHashMap<>();

        for (Denomination denom : denominations) {
            int count = (int) (balance / denom.getValue());
            if (count > 0) {
                balanceDistribution.put(denom, count);
                balance -= count * denom.getValue();
            }
        }
        return balanceDistribution;
    }
}
class POS {
    private BalanceCalculator balanceCalculator;

    public POS(BalanceCalculator balanceCalculator) {
        this.balanceCalculator = balanceCalculator;
    }

    public void processTransaction(double amountPaid, double totalCost) {
        if (amountPaid < totalCost) {
            System.out.println("Insufficient payment.");
            return;
        }

        double balance = amountPaid - totalCost;
        System.out.println("Total balance to return: $" + balance);

        Map<Denomination, Integer> balanceDistribution = balanceCalculator.calculateBalance(balance);

        System.out.println("Balance breakdown in USD:");
        for (Map.Entry<Denomination, Integer> entry : balanceDistribution.entrySet()) {
            System.out.println(entry.getValue() + " x " + entry.getKey().getName());
        }
    }
}
public class RetailPOSApplication {
    public static void main(String[] args) {
        BalanceCalculator balanceCalculator = new BalanceCalculator();
        POS pos = new POS(balanceCalculator);

        // Test case: Customer pays $200 for a $157.75 bill
        pos.processTransaction(200.0, 157.75);
    }
}
```
6. Tic tac toe in O(1)
    ```
          bool?[][] board; // X = true, O = false, empty space = null
    int[] row;
   int[] column;
   int diag = 0;       // Counter for main diagonal
   int antiDiag = 0;   // Counter for anti-diagonal

   bool XWon(int r, int c) {
    int boardSize = board.Length;

    // Ensure arrays are initialized based on board size
    if (row == null) row = new int[boardSize];
    if (column == null) column = new int[boardSize];

    // Check if it's X's turn
    if (board[r][c] != true) {
        throw new InvalidOperationException("Invalid move for X!");
    }

    // Update row, column, and diagonals
    row[r]++;
    column[c]++;
    if (r == c) diag++; // Main diagonal
    if (r + c == boardSize - 1) antiDiag++; // Anti-diagonal

    // Check if any condition for a win is satisfied
    if (row[r] == boardSize || column[c] == boardSize || diag == boardSize || antiDiag == boardSize) {
        return true; // X wins
    }

    return false; // X has not won yet
   

    ```
