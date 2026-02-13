const state = {
  profile: {
    big3Musicians: ["SZA", "Kendrick Lamar", "Frank Ocean"],
    big3Songs: ["Nights", "Good Days", "SAMIDOT"],
  },
  feed: [
    {
      postedBy: "Maya",
      song: "Ain't It Fun",
      artist: "Paramore",
      note: "Heard this in a thrift shop and forgot how hard it hits.",
    },
    {
      postedBy: "Theo",
      song: "Bags",
      artist: "Clairo",
      note: "If you like mellow late-night tracks, this one is perfect.",
    },
    {
      postedBy: "Noah",
      song: "Electric Feel",
      artist: "MGMT",
      note: "Found this again through an old FIFA playlist.",
    },
  ],
  currentIndex: 0,
  dms: [
    { fromMe: false, text: "Yo, dropped a new song in the feed!" },
    { fromMe: true, text: "Just saved it — great pick." },
  ],
  savedSongs: [],
};

const musiciansList = document.querySelector("#big3-musicians");
const songsList = document.querySelector("#big3-songs");
const songCard = document.querySelector("#song-card");
const skipBtn = document.querySelector("#skip-btn");
const saveBtn = document.querySelector("#save-btn");
const chatLog = document.querySelector("#chat-log");
const dmForm = document.querySelector("#dm-form");
const dmInput = document.querySelector("#dm-input");
const messageTemplate = document.querySelector("#chat-message-template");
const discoveryDialog = document.querySelector("#discovery-dialog");
const discoveryBtn = document.querySelector("#new-discovery-btn");
const cancelDialogBtn = document.querySelector("#cancel-dialog");
const discoveryForm = document.querySelector("#discovery-form");

function renderBig3() {
  musiciansList.innerHTML = "";
  songsList.innerHTML = "";

  state.profile.big3Musicians.forEach((artist) => {
    const item = document.createElement("li");
    item.textContent = artist;
    musiciansList.append(item);
  });

  state.profile.big3Songs.forEach((song) => {
    const item = document.createElement("li");
    item.textContent = song;
    songsList.append(item);
  });
}

function renderCurrentSong() {
  if (!state.feed.length) {
    songCard.innerHTML = "<h3>No songs yet</h3><p class='meta'>Follow more people to discover tracks.</p>";
    return;
  }

  const current = state.feed[state.currentIndex % state.feed.length];
  songCard.innerHTML = `
    <h3>${current.song} — ${current.artist}</h3>
    <p class="meta">Shared by @${current.postedBy}</p>
    <p class="note">“${current.note}”</p>
    <p class="meta">Saved by you: ${state.savedSongs.length}</p>
  `;
}

function renderMessages() {
  chatLog.innerHTML = "";

  state.dms.forEach((message) => {
    const node = messageTemplate.content.firstElementChild.cloneNode(true);
    node.classList.toggle("me", message.fromMe);
    node.querySelector(".bubble").textContent = message.text;
    chatLog.append(node);
  });

  chatLog.scrollTop = chatLog.scrollHeight;
}

function nextSong() {
  state.currentIndex = (state.currentIndex + 1) % state.feed.length;
  renderCurrentSong();
}

skipBtn.addEventListener("click", nextSong);

saveBtn.addEventListener("click", () => {
  const current = state.feed[state.currentIndex % state.feed.length];
  state.savedSongs.push(current);
  renderCurrentSong();
  nextSong();
});

dmForm.addEventListener("submit", (event) => {
  event.preventDefault();
  const text = dmInput.value.trim();
  if (!text) return;

  state.dms.push({ fromMe: true, text });
  dmInput.value = "";
  renderMessages();

  setTimeout(() => {
    state.dms.push({ fromMe: false, text: "Nice — let's queue that for tonight." });
    renderMessages();
  }, 500);
});

discoveryBtn.addEventListener("click", () => {
  discoveryDialog.showModal();
});

cancelDialogBtn.addEventListener("click", () => {
  discoveryDialog.close();
});

discoveryForm.addEventListener("submit", (event) => {
  event.preventDefault();
  const song = document.querySelector("#song-name").value.trim();
  const artist = document.querySelector("#artist-name").value.trim();
  const note = document.querySelector("#song-note").value.trim();

  if (!song || !artist || !note) return;

  state.feed.unshift({ postedBy: "you", song, artist, note });
  state.currentIndex = 0;
  renderCurrentSong();
  discoveryDialog.close();
  discoveryForm.reset();
});

renderBig3();
renderCurrentSong();
renderMessages();
