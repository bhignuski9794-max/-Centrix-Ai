# -Centrix-Ai
Personal Roadman AI assistant
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Centrix – Roadman AI</title>
  <style>
    body { font-family: sans-serif; max-width: 420px; margin: auto; padding: 20px; background: #f7f7f8; }
    #chat { height: 400px; overflow-y: scroll; border: 1px solid #ccc; padding: 10px; margin-bottom: 10px; border-radius: 8px; background: white; }
    .msg { margin: 8px 0; padding: 6px 12px; border-radius: 16px; max-width: 80%; display: inline-block; word-break: break-word; }
    .msg-bot { background: #e1f5fe; color: #0277bd; align-self: flex-start; }
    .msg-user { background: #c8e6c9; color: #388e3c; float: right; clear: both; }
    .msg img { max-width: 100%; border-radius: 8px; margin-top: 4px; }
    .clear { clear: both; }
    input, button { padding: 8px; margin-right: 4px; border-radius: 4px; border: 1px solid #ccc; }
    button { cursor: pointer; background: #0277bd; color: white; border: none; }
    button:disabled { background: #aaa; }
    small { display: block; margin-top: 10px; color: #555; }
  </style>
</head>
<body>
  <h1>Centrix – your roadman AI</h1>
  <div id="chat"></div>
  <input id="text-input" type="text" placeholder="Type something, fam..." />
  <button id="text-btn">Send</button>
  <button id="voice-btn">🎤 Speak</button>
  <small>“what’s 2 + 2”, “youtube cats”, “show me a dog”, “recall last”, “wagwan” … try them 😄</small>

  <script>
    // --- Centrix object – roadman chatbot ---
    const centrix = {
      name: 'Centrix',
      slang: true, // toggle roadman slang
      chatHistory: [], // in‑memory chat for this session only

      // All intents + responses. Each intent has one or more regex patterns and a response string/function.
      intents: [
        // Greetings
        { pattern: [/hi|hey|hello|wag1|wagwan/i], response: "Wagwan, fam 😄. I’m {name}." },
        { pattern: [/how are you|you alrite|you vibing/i], response: "I’m vibing proper, you own buzz, bruv?" },
        { pattern: [/your name/i], response: "My name’s {name}, roadman AI at your service, g." },
        { pattern: [/you mad|you buggin/i], response: "You buggin’, fam, chill 😂." },
        { pattern: [/joke|something funny/i], response: "Why did the roadman bring a ladder to the club? … ‘cause he heard the drinks were on the house, g 😄." },
        { pattern: [/bye|goodnight|later/i], response: "Later, bruv. Keep it 🔥." },
        // Math: basic arithmetic on two numbers
        { pattern: [/what(\'s| is) (\d+) (\+|\-|\*|\/) (\d+)/i],
          response: (match) => `${match[2]} ${match[3]} ${match[4]} is ${eval(match[2] + match[3] + match[4])}, fam` },
                                 
        { pattern: [/youtube (.+)/i],
          response: (match) => `Here’s youtube for **${match[1]}**, g: <a href="https:                                                                                                                   
        { pattern: [/google (.+)/i],
          response: (match) => `Can’t real‑time Google, bruv, but here’s a link: <a href="https>
          target="_blank">${match[1]}</a>, fam.` },
                                          
        { pattern: [/show me a dog|image of a dog/i], response: `<img src="https:                                                                
        { pattern: [/show me a cat|image of a cat/i], response: `<img src="https:                                                                   
        { pattern: [/show me a pizza|image of pizza/i], response: `<img src="https:                                                                              
                                                 
        { pattern: [/recall|what we said|last message/i],
          response: () => {
            if (centrix.chatHistory.length < 2) return "No past messages, bruv.";
            const last = centrix.chatHistory[centrix.chatHistory.length - 2];
            return `You said: "${last.user}". I said: "${last.bot}".`;
          }},
                   
        { pattern: [/.*/], response: "I don’t get that, fam. Try something simpler 😅." }
      ],

      formatResponse(r) {
        return r.replace(/{name}/g, this.name);
      },

      getReply(utterance) {
        utterance = this.slangify(utterance);                                   
        for (let intent of this.intents) {
          for (let p of intent.pattern) {
            if (p.test(utterance)) {
              const raw = typeof intent.response === 'function'
                ? intent.response(p.exec(utterance))
                : intent.response;
              const reply = this.formatResponse(raw);
                                        
              this.chatHistory.push({ user: utterance, bot: reply });
              return this.slangify(reply);
            }
          }
        }
      },

      slangify(phrase) {
        if (!this.slang) return phrase;
        let slangMap = [
          [/you are/g, 'you’re'],
          [/my name is (\w+)/g, '$1, innit'],
          [/hello/g, 'wagwan'],
          [/good/g, 'proppa'],
          [/man/g, 'bruv'],
          [/buddy/g, 'fam'],
          [/cool/g, 'sweet'],
          [/awesome/g, 'peak'],
        ];
        slangMap.forEach(([regex, sub]) => {
          phrase = phrase.replace(regex, sub);
        });
        return phrase;
      }
    };

                                         
    const chatDiv = document.getElementById('chat');
    const textInput = document.getElementById('text-input');
    const textBtn = document.getElementById('text-btn');
    const voiceBtn = document.getElementById('voice-btn');

    function addMessage(text, isBot = false) {
      let div = document.createElement('div');
      div.className = `msg ${isBot ? 'msg-bot' : 'msg-user'}`;
      div.innerHTML = text;
      
