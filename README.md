# Resumen-Biblia-Cada-Libro
cada libro de biblia resumen para ninos
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Random Joke Generator</title>
  <style>
    * { box-sizing: border-box; }

    body {
      min-height: 100vh;
      margin: 0;
      display: grid;
      place-items: center;
      padding: 24px;
      background: linear-gradient(135deg, #dbeafe, #fce7f3);
      color: #1f2937;
      font-family: system-ui, sans-serif;
    }

    main {
      width: min(100%, 520px);
      padding: 32px;
      text-align: center;
      background: white;
      border-radius: 20px;
      box-shadow: 0 16px 40px rgb(31 41 55 / 12%);
    }

    h1 { margin-top: 0; }

    #joke {
      min-height: 90px;
      margin: 24px 0;
      font-size: 1.2rem;
      line-height: 1.6;
    }

    button {
      border: 0;
      border-radius: 999px;
      padding: 12px 22px;
      background: #7c3aed;
      color: white;
      font: inherit;
      font-weight: 700;
      cursor: pointer;
    }

    button:hover { background: #6d28d9; }
    button:disabled { opacity: 0.6; cursor: wait; }

    #status {
      min-height: 1.5em;
      color: #6b7280;
      font-size: 0.9rem;
    }
  </style>
</head>
<body>
  <main>
    <h1>😂 Random Joke Generator</h1>
    <p id="joke" aria-live="polite">Click the button for a joke!</p>
    <p id="status" role="status"></p>
    <button id="jokeButton" type="button">Tell me a joke</button>
  </main>

  <script>
    const jokeElement = document.querySelector("#joke");
    const statusElement = document.querySelector("#status");
    const button = document.querySelector("#jokeButton");

    async function getJoke() {
      button.disabled = true;
      statusElement.textContent = "Finding a joke…";

      try {
        const response = await fetch(
          "https://v2.jokeapi.dev/joke/Any?safe-mode"
        );

        if (!response.ok) {
          throw new Error(`Request failed (${response.status})`);
        }

        const data = await response.json();

        if (data.error) {
          throw new Error("The joke service returned an error.");
        }

        jokeElement.textContent =
          data.type === "twopart"
            ? `${data.setup}\n\n${data.delivery}`
            : data.joke;

        statusElement.textContent = `Category: ${data.category}`;
      } catch (error) {
        jokeElement.textContent = "Sorry, I couldn't fetch a joke right now.";
        statusElement.textContent = error.message;
      } finally {
        button.disabled = false;
      }
    }

    button.addEventListener("click", getJoke);
  </script>
</body>
</html>
