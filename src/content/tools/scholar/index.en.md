---
title: "Scholar Mails Crawler"
date: 2025-03-02T23:41:22+07:00
draft: false
author: "AI4Code"
---

# Scholar Mails Crawler

A tool designed to automatically extract and organize academic publications from Google Scholar received via email. By parsing alert emails, this project collects metadata such as article titles and theirs links to help researchers stay updated with the latest publications in their field.

{{< tagsInput id="keywords" label="Keywords" >}}
{{< tagsInput id="topics" label="Topics" >}}

## Papers List

| Topic      | Branch     | Papers     |
| ---------- | ---------- | ---------- |
| Loading... | Loading... | Loading... |

<script type="text/javascript">
  async function fetchData() {
  const url = 'https://raw.githubusercontent.com/manhtdd/scholar_alters/master/data/papers.jsonl';
  try {
    const response = await fetch(url);
    const textData = await response.text(); // Read the response as text
    const lines = textData.trim().split('\n'); // Split the text into lines
    const data = lines.map(line => JSON.parse(line)); // Parse each line as JSON
    console.log(data);

    window.fetchedData = data;

    const table = document.querySelector('table tbody');
    table.innerHTML = '';

    data.forEach(paper => {
      const row = document.createElement('tr');
      row.innerHTML = `
        <td>${paper.first_label || 'N/A'}</td>
        <td>${paper.second_label || 'N/A'}</td>
        <td><a href="${paper.link}" target="_blank">${paper.title}</a></td>
      `;
      table.appendChild(row);
    });
  } catch (error) {
    console.error('Error fetching the data:', error);
  }
}

  // Search function to filter the table based on tags input
  function searchTable() {
    const keywords = document.getElementById('keywords').value.toLowerCase();
    const topics = document.getElementById('topics').value.toLowerCase();
    
    // Filter fetched data based on tags and rebuild the table
    const filteredData = window.fetchedData.filter(paper => {
      const matchesKeywords = keywords ? paper.title.toLowerCase().includes(keywords) : true;
      const matchesTopics = topics ? paper.topic.toLowerCase().includes(topics) : true;
      return matchesKeywords && matchesTopics;
    });

    // Rebuild the table with filtered data
    const table = document.querySelector('table tbody');
    table.innerHTML = '';  // Clear existing rows
    filteredData.forEach(paper => {
      const row = document.createElement('tr');
      row.innerHTML = `
        <td>${paper.first_label || 'N/A'}</td>
        <td>${paper.second_label || 'N/A'}</td>
        <td><a href="${paper.link}" target="_blank">${paper.title}</a></td>
      `;
      table.appendChild(row);
    });
  }

  // Call the fetchData function when the page loads
  document.addEventListener('DOMContentLoaded', () => {
    fetchData();

    // Add event listeners to the tags input fields for search
    document.getElementById('keywords').addEventListener('input', searchTable);
    document.getElementById('topics').addEventListener('input', searchTable);
  });
</script>
