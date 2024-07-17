# CipherSchools2024

1. Guessing a random number game-----------------------------------------------------------------------------------------------------------------------------------------------------------

#include <iostream>
#include <cstdlib>
#include <unordered_map>
#include <ctime>
#include <string>
#include <limits>

using namespace std;

unordered_map<string, int> scoreboard;

void clearInputBuffer() {
    cin.clear();
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
}

void playNumberGuessingGame() {
    string playerName;
    cout << "Please enter your name: ";
    cin >> playerName;
    
    srand(static_cast<unsigned int>(time(nullptr)));
    int secretNumber = rand() % 100 + 1;
    
    cout << "\n+-----------------------------------+" << endl;
    cout << "|   Welcome to Number Guessing Game  |" << endl;
    cout << "+-----------------------------------+" << endl;
    
    bool hasWon = false;
    const int maxAttempts = 6;
    int currentAttempt = 0;
    int playerGuess;
    
    while (!hasWon && currentAttempt < maxAttempts) {
        cout << "Attempt " << currentAttempt + 1 << "/" << maxAttempts << ": Enter your guess (1-100): ";
        cin >> playerGuess;
        currentAttempt++;
        
        if (playerGuess == secretNumber) {
            cout << "Congratulations, " << playerName << "! You've guessed the number in " << currentAttempt << " attempts." << endl;
            hasWon = true;
            scoreboard[playerName]++;
        } else {
            int difference = abs(secretNumber - playerGuess);
            if (playerGuess < secretNumber) {
                if (difference <= 5) cout << "A bit low, but very close!";
                else if (difference <= 15) cout << "Low, but you're getting warmer.";
                else cout << "Too low, try a higher number.";
            } else {
                if (difference <= 5) cout << "A bit high, but very close!";
                else if (difference <= 15) cout << "High, but you're getting warmer.";
                else cout << "Too high, try a lower number.";
            }
            cout << " Attempts left: " << maxAttempts - currentAttempt << endl;
        }
    }
    
    if (!hasWon) {
        cout << "Game over! The secret number was " << secretNumber << "." << endl;
    }
}

void showLeaderboard() {
    cout << "\n--- Leaderboard ---" << endl;
    cout << "Player\t\tWins" << endl;
    cout << "-----------------" << endl;
    
    for (const auto &player : scoreboard) {
        cout << player.first << "\t\t" << player.second << endl;
    }
    cout << endl;
}

void displayGameMenu() {
    int userChoice;
    do {
        cout << "\nGame Menu:" << endl;
        cout << "1. Start New Game" << endl;
        cout << "2. View Leaderboard" << endl;
        cout << "3. Quit" << endl;
        cout << "Enter your choice (1-3): ";
        
        while (!(cin >> userChoice)) {
            clearInputBuffer();
            cout << "Invalid input. Please enter a number (1-3): ";
        }
        
        switch (userChoice) {
            case 1:
                playNumberGuessingGame();
                break;
            case 2:
                showLeaderboard();
                break;
            case 3:
                cout << "Thank you for playing. Goodbye!" << endl;
                break;
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    } while (userChoice != 3);
}

int main() {
    displayGameMenu();
    return 0;
}


2. To do list------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


#include <iostream>
#include <map>
#include <set>
#include <vector>
#include <string>

using namespace std;

class User {
public:
    string firstName;
    string lastName;
    int age;
    string gender;

    User() {} ;

    User(string firstName, string lastName, int age, string gender) {
        this->firstName = firstName;
        this->lastName = lastName;
        this->age = age;
        this->gender = gender;
    }
};


map<string, User> mapUserName;

map<string, set<string>> Friends;

class Message {
public:
    string sender;
    string receiver;
    string content;

    Message(string sender, string receiver, string content) {
        this->sender = sender;
        this->receiver = receiver;
        this->content = content;
    }
};

class Group {
public:
    string groupName;
    set<string> members;

    Group() {} // Default constructor

    Group(string groupName) {
        this->groupName = groupName;
    }

    void addMember(string userName) {
        members.insert(userName);
    }

    void displayMembers() {
        cout << "Group " << groupName << " members: ";
        for (const auto& member : members) {
            cout << member << " ";
        }
        cout << endl;
    }
};

map<string, vector<Message>> userMessages; // MAP user to their messages
map<string, Group> groups; // MAP group name to Group

void addUser(string userName, User user) {
    if (mapUserName.find(userName) == mapUserName.end()) {
        mapUserName[userName] = user;
    } else {
        cout << "UserName Already Taken" << endl;
    }
}

void makeThemFriend(string userName1, string userName2) {
    Friends[userName1].insert(userName2); // mapping 1st friend to another
    Friends[userName2].insert(userName1);
}

void displayAllUsers() {
    for (auto i : mapUserName) {
        cout << "UserName: " << i.first << ", Name: " << i.second.firstName << " " << i.second.lastName << "\n";
    }
}

void displayAllFriendships() {
    for (auto i : Friends) {
        cout << i.first << ":" << "\n";
        set<string> friends = i.second;
        for (auto j : friends) {
            cout << j << " ";
        }
        cout << "\n";
    }
}

void sendMessage(string sender, string receiver, string content) {
    Message message(sender, receiver, content);
    userMessages[receiver].push_back(message);
}

void displayUserMessages(string userName) {
    if (userMessages.find(userName) != userMessages.end()) {
        for (const auto& message : userMessages[userName]) {
            cout << "From: " << message.sender << ", Message: " << message.content << "\n";
        }
    } else {
        cout << "No messages for " << userName << endl;
    }
}

int main() {
    User alice("Alice", "Mishra", 30, "Female");
    User bob("Bob", "Ali", 27, "Male");
    User charlie("Charlie", "Brown", 25, "Male");

    addUser("Alice", alice);
    addUser("Bob", bob);
    addUser("Charlie", charlie);

    makeThemFriend("Alice", "Bob");
    makeThemFriend("Alice", "Charlie");

    displayAllUsers();
    displayAllFriendships();

    sendMessage("Alice", "Bob", "Hello Bob!");
    sendMessage("Bob", "Alice", "Hi Alice!");

    cout << "\nMessages for Bob:" << endl;
    displayUserMessages("Bob");

    cout << "\nMessages for Alice:" << endl;
    displayUserMessages("Alice");


    Group group1("Friends");
    group1.addMember("Alice");
    group1.addMember("Bob");

    groups["Friends"] = group1;

    cout << "\nGroups:" << endl;
    for (auto& group : groups) {
        group.second.displayMembers();
    }

    // User input to add more users and friendships
    string userName, firstName, lastName, gender, user1, user2;
    int age;
    char choice;

    do {
        cout << "\nEnter new user details:\n";
        cout << "UserName: ";
        cin >> userName;
        cout << "First Name: ";
        cin >> firstName;
        cout << "Last Name: ";
        cin >> lastName;
        cout << "Age: ";
        cin >> age;
        cout << "Gender: ";
        cin >> gender;

        addUser(userName, User(firstName, lastName, age, gender));

        cout << "Do you want to add another user? (y/n): ";
        cin >> choice;
    } while (choice == 'y');

    do {
        cout << "\nEnter friendship details:\n";
        cout << "User 1: ";
        cin >> user1;
        cout << "User 2: ";
        cin >> user2;

        makeThemFriend(user1, user2);

        cout << "Do you want to add another friendship? (y/n): ";
        cin >> choice;
    } while (choice == 'y');

    cout << "\nUpdated Friendships:\n";
    displayAllFriendships();

    return 0;
}

3. Social media app------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#include <iostream>
#include <vector>
#include <string>
#include <ctime>

using namespace std;

struct Task {
    string description;
    string category;
    bool completed;
    string dueDate;
    string priority;

    Task(string desc, string cat, string prio, string due) : 
        description(desc), category(cat), completed(false), dueDate(due), priority(prio) {}
};

vector<Task> taskList;
vector<string> categories = {"Work", "Personal", "Study", "Errands", "Other"};
Task lastDeletedTask("", "", "", "");
bool taskDeleted = false;

void displayTasks() {
    if (taskList.empty()) {
        cout << "No tasks to display. Add some tasks first.\n";
        return;
    }
    cout << "\nTasks:\n";
    for (size_t i = 0; i < taskList.size(); ++i) {
        cout << i + 1 << ". " << taskList[i].description << " | "
             << taskList[i].category << " | " << taskList[i].priority << " | "
             << taskList[i].dueDate << " | " 
             << (taskList[i].completed ? "Done" : "Pending") << "\n";
    }
}

void addTask() {
    string description, category, priority;
    cout << "Enter task description: ";
    cin.ignore();
    getline(cin, description);

    cout << "Choose category:\n";
    for (size_t i = 0; i < categories.size(); ++i) {
        cout << i + 1 << ". " << categories[i] << endl;
    }
    int categoryChoice;
    cin >> categoryChoice;
    category = (categoryChoice > 0 && categoryChoice <= categories.size()) 
               ? categories[categoryChoice - 1] : "Other";

    cout << "Enter priority (HIGH/MEDIUM/LOW): ";
    cin >> priority;

    time_t now = time(0);
    char* dt = ctime(&now);
    string dueDate(dt);
    dueDate.resize(dueDate.length() - 1);  // Remove newline

    taskList.push_back(Task(description, category, priority, dueDate));
    cout << "Task added successfully.\n";
}

void markTaskCompleted() {
    int taskNumber;
    cout << "Enter task number to mark as completed: ";
    cin >> taskNumber;

    if (taskNumber > 0 && taskNumber <= taskList.size()) {
        taskList[taskNumber - 1].completed = true;
        cout << "Task marked as completed.\n";
    } else {
        cout << "Invalid task number.\n";
    }
}

void deleteTask() {
    int taskNumber;
    cout << "Enter task number to delete: ";
    cin >> taskNumber;

    if (taskNumber > 0 && taskNumber <= taskList.size()) {
        lastDeletedTask = taskList[taskNumber - 1];
        taskList.erase(taskList.begin() + taskNumber - 1);
        taskDeleted = true;
        cout << "Task deleted.\n";
    } else {
        cout << "Invalid task number.\n";
    }
}

void undoDelete() {
    if (taskDeleted) {
        taskList.push_back(lastDeletedTask);
        taskDeleted = false;
        cout << "Last deleted task restored.\n";
    } else {
        cout << "No task to restore.\n";
    }
}

void showMenu() {
    int choice;
    do {
        cout << "\nTask Manager Menu:\n"
             << "1. Add Task\n"
             << "2. Display Tasks\n"
             << "3. Mark Task as Completed\n"
             << "4. Delete Task\n"
             << "5. Undo Last Delete\n"
             << "6. Exit\n"
             << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: addTask(); break;
            case 2: displayTasks(); break;
            case 3: markTaskCompleted(); break;
            case 4: deleteTask(); break;
            case 5: undoDelete(); break;
            case 6: cout << "Exiting. Goodbye!\n"; break;
            default: cout << "Invalid choice. Try again.\n";
        }
    } while (choice != 6);
}

int main() {
    showMenu();
    return 0;
}
