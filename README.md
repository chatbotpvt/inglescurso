<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Chatbot para Aprender Inglés</title>
  <style>
    body { font-family: Arial; background: #f1f1f1; margin: 0; padding: 0; }
    .chat-container {
      max-width: 600px;
      margin: 40px auto;
      background: #fff;
      border-radius: 10px;
      padding: 20px;
      box-shadow: 0 0 10px rgba(0,0,0,0.2);
    }
    .chat-box { height: 400px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; margin-bottom: 10px; }
    .user { color: blue; font-weight: bold; }
    .bot { color: green; font-weight: bold; }
    input, button {
      padding: 10px;
      font-size: 16px;
    }
    button { margin-left: 5px; }
  </style>
</head>
<body>
  <div class="chat-container">
    <h2>🤖 Chatbot de Inglés</h2>
    <div class="chat-box" id="chat-box"></div>
    <input type="text" id="user-input" placeholder="Escribe en español o pregunta algo..." />
    <button onclick="sendMessage()">Enviar</button>
  </div>

  <script>
    const chatBox = document.getElementById('chat-box');
    const input = document.getElementById('user-input');

    async function sendMessage() {
      const userMessage = input.value.trim();
      if (!userMessage) return;

      chatBox.innerHTML += `<div><span class="user">Tú:</span> ${userMessage}</div>`;
      input.value = '';

      const response = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": "Bearer TU_API_KEY" // ← REEMPLAZA AQUÍ
        },
        body: JSON.stringify({
          model: "gpt-3.5-turbo",
          messages: [
            { role: "system", content: "Eres un profesor de inglés amigable que enseña con ejemplos, frases útiles y conversación básica." },
            { role: "user", content: userMessage }
          ]
        })
      });

      const data = await response.json();
      const botReply = data.choices?.[0]?.message?.content || "Lo siento, hubo un error.";

      chatBox.innerHTML += `<div><span class="bot">Chatbot:</span> ${botReply}</div>`;
      chatBox.scrollTop = chatBox.scrollHeight;
    }
  </script>
</body>
</html>
