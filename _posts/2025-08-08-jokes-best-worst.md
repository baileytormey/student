---
toc: true
layout: post
title: Best and Worst Jokes
permalink: /best-worst
description: Best and Worst Jokes
type: ccc
---

<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Best & Worst Jokes</title>
  <style>
    body {
      font-family: "Segoe UI", Tahoma, sans-serif;
      padding: 20px;
      color: white;
      background: #111; /* Dark background for contrast */
    }
    h1 {
      text-align: center;
      font-size: 2rem;
      margin-bottom: 30px;
    }
    h2 {
      margin-bottom: 10px;
      font-size: 1.3rem;
    }
    button {
      padding: 10px 20px;
      font-size: 1rem;
      border: none;
      border-radius: 25px;
      cursor: pointer;
      color: white;
      margin: 8px 0;
      transition: all 0.3s ease;
    }
    #fetchBtn1 {
      background: linear-gradient(135deg, #28a745, #4dd17e);
    }
    #fetchBtn1:hover {
      transform: scale(1.05);
      background: linear-gradient(135deg, #1e7e34, #3bbf6b);
    }
    #fetchBtn2 {
      background: linear-gradient(135deg, #dc3545, #f1737b);
    }
    #fetchBtn2:hover {
      transform: scale(1.05);
      background: linear-gradient(135deg, #b02a37, #e85c65);
    }
    .joke-box {
      border: 2px dashed rgba(255,255,255,0.5);
      border-radius: 10px;
      padding: 12px;
      margin-top: 10px;
      color: white;
      font-size: 1.1rem;
      line-height: 1.4;
      opacity: 0;
      transform: translateY(10px);
      transition: all 0.4s ease;
    }
    .joke-box.show {
      opacity: 1;
      transform: translateY(0);
    }
    .loading {
      opacity: 0.7;
    }
    .joke-section {
      margin-bottom: 25px;
    }
  </style>
</head>
<body>

  <div class="joke-section">
    <h2> Best Joke</h2>
    <button id="fetchBtn1">Tell me the best</button>
    <div id="result1" class="joke-box"></div>
  </div>

  <div class="joke-section">
    <h2>💀 Worst Joke</h2>
    <button id="fetchBtn2">Tell me the worst</button>
    <div id="result2" class="joke-box"></div>
  </div>

  <script>
    async function fetchJokes(url, btnId, resultId) {
      const btn = document.getElementById(btnId);
      const result = document.getElementById(resultId);

      btn.disabled = true;
      btn.classList.add('loading');
      btn.textContent = 'Loading...';
      result.classList.remove('show');
      result.textContent = '';

      try {
        const response = await fetch(url);
        if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);

        const data = await response.json();
        result.textContent = data.joke || '(No joke found in response)';
        
        // trigger animation
        setTimeout(() => result.classList.add('show'), 10);

      } catch (err) {
        result.textContent = `Error: ${err.message}`;
        result.classList.add('show');
      } finally {
        btn.disabled = false;
        btn.classList.remove('loading');
        btn.textContent = btnId === 'fetchBtn1' ? 'Tell me the best' : 'Tell me the worst';
      }
    }

    document.getElementById('fetchBtn1').addEventListener('click', () => {
      fetchJokes('http://127.0.0.1:8587/api/jokes/best', 'fetchBtn1', 'result1');
    });

    document.getElementById('fetchBtn2').addEventListener('click', () => {
      fetchJokes('http://127.0.0.1:8587/api/jokes/worst', 'fetchBtn2', 'result2');
    });
  </script>

</body>
</html>
