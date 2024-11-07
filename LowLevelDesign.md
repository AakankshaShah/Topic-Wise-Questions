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
