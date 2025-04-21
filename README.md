<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>iPad 剪贴板笔记</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    textarea:focus, input:focus { outline: none; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-4">
  <div class="max-w-2xl mx-auto">
    <h1 class="text-2xl font-bold mb-4 text-center">📋 我的剪贴板笔记</h1>

    <div class="mb-4">
      <input id="noteInput" type="text" placeholder="输入你的笔记内容..." class="w-full p-3 rounded-xl border border-gray-300 shadow-sm focus:ring focus:ring-blue-200" />
      <button onclick="submitNote()" class="mt-2 w-full bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded-xl">添加笔记</button>
    </div>

    <div id="notesContainer" class="space-y-4"></div>

    <div class="mt-6 flex justify-center">
      <button onclick="clearNotes()" class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full">清除所有笔记</button>
    </div>
  </div>

  <script>
    const notesContainer = document.getElementById('notesContainer');
    const noteInput = document.getElementById('noteInput');

    function renderNotes() {
      notesContainer.innerHTML = '';
      const notes = JSON.parse(localStorage.getItem('ipadNotes') || '[]');
      notes.forEach((note, index) => {
        const noteEl = document.createElement('div');
        noteEl.className = 'bg-white p-4 rounded-xl shadow border border-gray-300 whitespace-pre-wrap';
        noteEl.textContent = note;
        notesContainer.appendChild(noteEl);
      });
    }

    function saveNote(text) {
      const notes = JSON.parse(localStorage.getItem('ipadNotes') || '[]');
      notes.unshift(text);
      localStorage.setItem('ipadNotes', JSON.stringify(notes));
      renderNotes();
    }

    function submitNote() {
      const text = noteInput.value.trim();
      if (text) {
        saveNote(text);
        noteInput.value = '';
      }
    }

    function clearNotes() {
      if (confirm('你确定要清除所有笔记吗？')) {
        localStorage.removeItem('ipadNotes');
        renderNotes();
      }
    }

    window.addEventListener('paste', (event) => {
      const pastedText = (event.clipboardData || window.clipboardData).getData('text');
      if (pastedText.trim()) {
        saveNote(pastedText.trim());
      }
    });

    window.addEventListener('DOMContentLoaded', renderNotes);
  </script>
</body>
</html>
