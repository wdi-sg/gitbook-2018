for (var i=0, link; i<5; i++) {

  link = document.createElement("a");

  link.innerHTML = "Link " + i;

  link.addEventListener = function () {
    alert(i);
  };

  document.body.appendChild(link);
}

morning exercise: https://github.com/wdi-sg/event-dom-practice

lecture: dom manip

afternoon: tictactoe run through
