```
pandoc "я_практикум_архитектур.md" `
-o "я_практикум_архитектур.html" `
-f markdown -t html5 --standalone --metadata title="Архитектура Программного обеспечения - Яндекс.Практикум "
```

```
pandoc "я_практикум_архитектур.md" `
-o out.html `
-f markdown -t html5 `
--standalone `
--metadata title="Архитектура Программного обеспечения - Яндекс.Практикум" `
-o "Архитектура ПО ЯПрактикум.html" 
```


- меняем
  - Удалить старое оглавление
    - вставить после header 
    ```
        <header id="title-block-header">
        <h1 class="title">Архитектура Программного обеспечения - Яндекс.Практикум</h1>
        </header>
        <nav id="TOC">
        <h2>Оглавление</h2>
        <ul id="toc-list"></ul>
        </nav>    
    ```
  - Кнопка "наверх"
    - Добавить перед </body>
      - <button id="scrollTopBtn">↑</button>
    - CSS кнопки (если нет)
      ```
            #scrollTopBtn {
              position: fixed;
              bottom: 20px;
              left: 20px;
              z-index: 9999;
              padding: 10px 14px;
              font-size: 18px;
              border-radius: 8px;
              border: none;
              background: #1a1a1a;
              color: #fff;
              cursor: pointer;
              opacity: 0.6;
              display:none;
            }
    
            #scrollTopBtn:hover {
              opacity:1;
            }      
      ```
      
  - JS генерация оглавления + кнопка вверх
    -  Добавить перед </body>
```
<script>

document.addEventListener("DOMContentLoaded", () => {

  const toc = document.getElementById("toc-list");
  const headers = document.querySelectorAll("h2, h3, h4");

  let currentH2;
  let currentH3;

  headers.forEach(h => {

    if (!h.id) {
      h.id = h.textContent
        .toLowerCase()
        .replace(/[^\wа-яё]+/g,"-")
        .replace(/^-|-$/g,"");
    }

    const li = document.createElement("li");
    const a = document.createElement("a");

    a.textContent = h.textContent;
    a.href = "#" + h.id;

    li.appendChild(a);

    if (h.tagName === "H2") {

      currentH2 = document.createElement("ul");

      const root = document.createElement("li");
      root.appendChild(a);
      root.appendChild(currentH2);

      toc.appendChild(root);

      currentH3 = null;
    }

    else if (h.tagName === "H3") {

      currentH3 = document.createElement("ul");

      const item = document.createElement("li");
      item.appendChild(a);
      item.appendChild(currentH3);

      currentH2.appendChild(item);
    }

    else if (h.tagName === "H4") {

      const item = document.createElement("li");
      item.appendChild(a);

      if (currentH3) currentH3.appendChild(item);
    }

  });


  const btn = document.getElementById("scrollTopBtn");

  window.addEventListener("scroll", () => {

    if (document.documentElement.scrollTop > 300) {
      btn.style.display = "block";
    } else {
      btn.style.display = "none";
    }

  });

  btn.addEventListener("click", () => {

    window.scrollTo({
      top: 0,
      behavior: "smooth"
    });

  });

});

</script>
```

    