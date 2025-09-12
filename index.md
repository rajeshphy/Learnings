---
layout: default
title: Free Press Segmented
---


<div id="button-sections" style="padding: 1rem;"></div>

<script>
fetch('{{ site.baseurl }}/url.txt')
  .then(response => response.text())
  .then(data => {
    const lines = data.trim().split('\n');
    const container = document.getElementById('button-sections');
    let section = null;

    lines.forEach(line => {
      if (line.startsWith('---') && line.endsWith('---')) {
        const category = line.replace(/---/g, '').trim();
        section = document.createElement('div');
        section.className = 'category-section';

        const heading = document.createElement('h2');
        heading.textContent = category;
        heading.className = 'category-title';
        section.appendChild(heading);

        const buttonContainer = document.createElement('div');
        buttonContainer.className = 'button-group';
        section.appendChild(buttonContainer);

        container.appendChild(section);
      } else if (section && line.startsWith('http')) {
        const parts = line.split(',');
        const url = parts[0].trim();
        const label = parts[1] ? parts[1].trim() : new URL(url).hostname.replace('www.', '');

        const btn = document.createElement('button');
        btn.textContent = label;
        btn.className = 'news-button';
        btn.onclick = () => window.open(url, '_blank');

        section.querySelector('.button-group').appendChild(btn);
      }
    });
  });
</script>

<style>
/* Global responsiveness */
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: 'Inter', sans-serif;
  background: #f8f9fa;
  color: #212529;
}

/* Section styling */
.category-section {
  margin-bottom: 2rem;
  padding: 0.5rem;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 3px 8px rgba(0,0,0,0.08);
}

.category-title {
  font-size: 1.3rem;
  margin: 1rem 0 0.5rem;
  padding-left: 0.5rem;
  border-left: 4px solid #5a8dee;
}

/* Buttons */
.button-group {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  padding: 1rem;
  justify-content: flex-start;
}

.news-button {
  flex: 1 1 160px;
  padding: 0.7rem 1rem;
  border: none;
  border-radius: 10px;
  background: linear-gradient(to right, #5a8dee, #6fcf97);
  color: white;
  font-size: 1rem;
  font-weight: 600;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.news-button:hover {
  transform: translateY(-2px);
  background: linear-gradient(to right, #6fcf97, #5a8dee);
}

/* Mobile tweaks */
@media (max-width: 600px) {
  .news-button {
    flex: 1 1 100%;
    font-size: 1rem;
  }

  .category-title {
    font-size: 1.1rem;
  }
}
</style>