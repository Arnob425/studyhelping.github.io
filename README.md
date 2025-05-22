WELCOME
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>AB AI Chatbot</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background-color: #121212;
      color: #ffffff;
      display: flex;
      flex-direction: column;
      height: 100vh;
    }

    header {
      background-color: #1f1f1f;
      padding: 20px;
      text-align: center;
      font-size: 28px;
      color: #00bcd4;
    }

    #chatbox {
      flex: 1;
      padding: 20px;
      overflow-y: auto;
    }

    .user, .bot {
      margin-bottom: 15px;
      padding: 10px;
      border-radius: 8px;
      max-width: 70%;
    }

    .user {
      background-color: #007bff;
      align-self: flex-end;
      text-align: right;
    }

    .bot {
      background-color: #333;
      align-self: flex-start;
      text-align: left;
    }

    #inputArea {
      display: flex;
      padding: 15px;
      background-color: #1f1f1f;
    }

    input {
      flex: 1;
      padding: 10px;
      border: none;
      border-radius: 6px;
      font-size: 16px;
      margin-right: 10px;
    }

    button {
      padding: 10px 15px;
      background-color: #00bcd4;
      border: none;
      border-radius: 6px;
      color: white;
      font-size: 16px;
      cursor: pointer;
    }

    .footer {
      position: fixed;
      bottom: 5px;
      right: 10px;
      font-size: 14px;
      color: #ff9800;
      font-weight: bold;
    }
  </style>
</head>
<body>

  <header>AB AI - Ask Anything, Learn Everything</header>

  <div id="chatbox"></div>

  <div id="inputArea">
    <input type="text" id="userInput" placeholder="Type your question..." />
    <button onclick="sendMessage()">Send</button>
  </div>

  <div class="footer">Made by Arnob</div>

  <script>
    const chatbox = document.getElementById('chatbox');

    async function sendMessage() {
      const inputField = document.getElementById('userInput');
      const userText = inputField.value.trim();
      if (!userText) return;

      showMessage(userText, 'user');
      inputField.value = '';

      const reply = await getGPTReply(userText);
      showMessage(reply, 'bot');
    }

    function showMessage(message, sender) {
      const msgDiv = document.createElement('div');
      msgDiv.className = sender;
      msgDiv.textContent = message;
      chatbox.appendChild(msgDiv);
      chatbox.scrollTop = chatbox.scrollHeight;
    }

    async function getGPTReply(prompt) {
      const apiKey =  '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';
      const url = 'https://api.openai.com/v1/chat/completions';

      try {
        const response = await fetch(url, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${apiKey}`
          },
          body: JSON.stringify({
            model: 'gpt-3.5-turbo',
            messages: [{ role: 'user', content: prompt }]
          })
        });

        const data = await response.json();
        return data.choices[0].message.content.trim();
      } catch (err) {
        return 'waiting.';
      }
    }
  </script>
</body>
</html>
