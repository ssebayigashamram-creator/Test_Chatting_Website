# Test_Chatting_Website
Sample Chatting Website created using ChatGpt
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Friends Chat</title>

    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
        }

        body {
            background: #e9eef3;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .chat-app {
            width: 95%;
            max-width: 1000px;
            height: 90vh;
            background: white;
            display: flex;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 5px 25px rgba(0,0,0,0.2);
        }

        /* Friends */
        .friends {
            width: 30%;
            background: #17212b;
            color: white;
            display: flex;
            flex-direction: column;
        }

        .friends-header {
            padding: 20px;
            font-size: 22px;
            font-weight: bold;
            background: #202b36;
        }

        .search {
            margin: 12px;
            padding: 10px;
            border-radius: 8px;
            border: none;
            width: calc(100% - 24px);
            outline: none;
        }

        .friend {
            padding: 15px;
            display: flex;
            align-items: center;
            gap: 12px;
            cursor: pointer;
        }

        .friend:hover {
            background: #263542;
        }

        .avatar {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background: #3498db;
            display: flex;
            justify-content: center;
            align-items: center;
            font-weight: bold;
        }

        .online {
            color: #2ecc71;
            font-size: 12px;
        }

        /* Chat */
        .chat {
            width: 70%;
            display: flex;
            flex-direction: column;
        }

        .chat-header {
            height: 70px;
            background: #3498db;
            color: white;
            display: flex;
            align-items: center;
            padding: 15px 20px;
            font-size: 20px;
            font-weight: bold;
        }

        .messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            background: #f5f7f9;
        }

        .message {
            max-width: 70%;
            padding: 10px 14px;
            margin-bottom: 12px;
            border-radius: 12px;
            clear: both;
            word-wrap: break-word;
        }

        .received {
            background: white;
            float: left;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
        }

        .sent {
            background: #3498db;
            color: white;
            float: right;
        }

        .time {
            font-size: 10px;
            opacity: 0.7;
            margin-top: 5px;
            text-align: right;
        }

        .input-area {
            display: flex;
            padding: 12px;
            background: white;
            border-top: 1px solid #ddd;
            gap: 8px;
        }

        .input-area input {
            flex: 1;
            padding: 13px;
            border: 1px solid #ccc;
            border-radius: 25px;
            outline: none;
            font-size: 15px;
        }

        .send-btn {
            border: none;
            background: #3498db;
            color: white;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 18px;
        }

        .send-btn:hover {
            background: #2980b9;
        }

        @media (max-width: 700px) {
            .chat-app {
                width: 100%;
                height: 100vh;
                border-radius: 0;
            }

            .friends {
                width: 35%;
            }

            .chat {
                width: 65%;
            }

            .friends-header {
                font-size: 16px;
            }

            .friend {
                padding: 10px;
            }

            .avatar {
                width: 35px;
                height: 35px;
            }
        }
    </style>
</head>

<body>

<div class="chat-app">

    <!-- Friends list -->
    <div class="friends">

        <div class="friends-header">
            💬 My Chat
        </div>

        <input
            class="search"
            type="text"
            placeholder="Search friends..."
            id="search"
        >

        <div id="friendsList">

            <div class="friend" onclick="selectFriend('John')">
                <div class="avatar">J</div>
                <div>
                    <strong>John</strong>
                    <div class="online">● Online</div>
                </div>
            </div>

            <div class="friend" onclick="selectFriend('Sarah')">
                <div class="avatar">S</div>
                <div>
                    <strong>Sarah</strong>
                    <div class="online">● Online</div>
                </div>
            </div>

            <div class="friend" onclick="selectFriend('David')">
                <div class="avatar">D</div>
                <div>
                    <strong>David</strong>
                    <div class="online">● Online</div>
                </div>
            </div>

        </div>
    </div>

    <!-- Chat area -->
    <div class="chat">

        <div class="chat-header" id="chatHeader">
            John
        </div>

        <div class="messages" id="messages">
            <div class="message received">
                Hey! 👋
                <div class="time">12:00</div>
            </div>

            <div class="message received">
                How are you?
                <div class="time">12:01</div>
            </div>

            <div class="message sent">
                I'm fine! 😊
                <div class="time">12:02</div>
            </div>
        </div>

        <div class="input-area">

            <input
                type="text"
                id="messageInput"
                placeholder="Type a message..."
                autocomplete="off"
            >

            <button class="send-btn" onclick="sendMessage()">
                ➤
            </button>

        </div>

    </div>

</div>

<script>

    let currentFriend = "John";

    function selectFriend(friend) {

        currentFriend = friend;

        document.getElementById("chatHeader").textContent = friend;

        document.getElementById("messages").innerHTML = "";

        const savedMessages =
            JSON.parse(localStorage.getItem("chat_" + friend)) || [];

        if (savedMessages.length === 0) {

            addMessage(
                "Hi! 👋",
                "received"
            );

        } else {

            savedMessages.forEach(function(message) {

                addMessage(
                    message.text,
                    message.type,
                    message.time
                );

            });

        }

    }


    function sendMessage() {

        const input =
            document.getElementById("messageInput");

        const text = input.value.trim();

        if (text === "") {
            return;
        }

        const now = new Date();

        const time =
            now.getHours().toString().padStart(2, "0")
            + ":" +
            now.getMinutes().toString().padStart(2, "0");

        addMessage(text, "sent", time);

        saveMessage(text, "sent", time);

        input.value = "";

        setTimeout(function() {

            addMessage(
                "I received your message 👍",
                "received"
            );

        }, 1000);
    }


    function addMessage(text, type, time) {

        const messages =
            document.getElementById("messages");

        const message =
            document.createElement("div");

        message.className =
            "message " + type;

        if (!time) {

            const now = new Date();

            time =
                now.getHours().toString().padStart(2, "0")
                + ":" +
                now.getMinutes().toString().padStart(2, "0");
        }

        message.innerHTML =
            text +
            '<div class="time">' +
            time +
            "</div>";

        messages.appendChild(message);

        messages.scrollTop =
            messages.scrollHeight;
    }


    function saveMessage(text, type, time) {

        const key = "chat_" + currentFriend;

        const messages =
            JSON.parse(localStorage.getItem(key)) || [];

        messages.push({
            text: text,
            type: type,
            time: time
        });

        localStorage.setItem(
            key,
            JSON.stringify(messages)
        );
    }


    document
        .getElementById("messageInput")
        .addEventListener("keydown", function(event) {

            if (event.key === "Enter") {
                sendMessage();
            }

        });


    document
        .getElementById("search")
        .addEventListener("input", function() {

            const search =
                this.value.toLowerCase();

            const friends =
                document.querySelectorAll(".friend");

            friends.forEach(function(friend) {

                const name =
                    friend.innerText.toLowerCase();

                if (name.includes(search)) {
                    friend.style.display = "flex";
                } else {
                    friend.style.display = "none";
                }

            });

        });

</script>

</body>
</html>
