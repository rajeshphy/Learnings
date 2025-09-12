---
layout: default
title: Notes Arxived
---

<div id="button-sections" style="padding: 1rem;"></div>

<script>
fetch('{{ site.baseurl }}/learn.txt')
  .then(response => response.text())
  .then(data => {
    const lines = data.trim().split('\n');
    const container = document.getElementById('button-sections');
    let section = null;
    let colorIndex = 0;
    const colors = ['#1a73e8', '#188038']; // blue, green

    lines.forEach(line => {
      line = line.trim();
      if (!line) return;

      // Handle category headings
      if (line.startsWith('---') && line.endsWith('---')) {
        const category = line.replace(/---/g, '').trim();
        section = document.createElement('div');
        section.className = 'category-section';

        const heading = document.createElement('h2');
        heading.textContent = category;
        heading.className = 'category-title';
        section.appendChild(heading);

        const contentContainer = document.createElement('div');
        contentContainer.className = 'link-group';
        section.appendChild(contentContainer);

        container.appendChild(section);
      } 
      // % lines: centered shaded heading-style blocks
      else if (section && line.startsWith('%')) {
        const shadedHeading = document.createElement('div');
        shadedHeading.textContent = line.slice(1).trim();
        shadedHeading.className = 'shaded-heading';
        section.querySelector('.link-group').appendChild(shadedHeading);
      }
      // Hyperlinks: alternate blue and green
      else if (section && line.startsWith('http')) {
        const parts = line.split(',');
        const url = parts[0].trim();
        const label = parts[1] ? parts[1].trim() : new URL(url).hostname.replace('www.', '');

        const link = document.createElement('a');
        link.href = url;
        link.textContent = label;
        link.target = '_blank';
        link.style.fontWeight = 'bold';
        link.style.fontFamily = '"Times New Roman", Times, serif';
        link.style.color = colors[colorIndex % colors.length];
        link.style.display = 'block';
        link.style.marginBottom = '0.5rem';

        section.querySelector('.link-group').appendChild(link);
        colorIndex++;
      }
    });
  });
</script>

<style>
body {
  margin: 0;
  font-family: 'Inter', sans-serif;
  background: #f8f9fa;
  color: #212529;
}

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

.link-group {
  padding: 1rem;
}

/* Center-aligned shaded heading for % lines */
.shaded-heading {
  font-weight: bold;
  font-family: "Times New Roman", Times, serif;
  color: #000000;
  background-color: #e9ecef;
  padding: 0.4rem 0.6rem;
  margin-bottom: 0.6rem;
  border-radius: 6px;
  font-size: 1.1rem;
  box-shadow: inset 0 1px 3px rgba(0,0,0,0.05);
  text-align: center;
}
</style>