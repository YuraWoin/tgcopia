# tgcopia 💬

**LogiTalk** — a simple online chat built with **Python + CustomTkinter + sockets**.

The project has two parts: a TCP server that relays messages to every connected
client, and a GUI client that connects to it and renders the chat.

> Status: **beta**. It works, but still needs polish (security, protocol, chat history).

---

## ✨ Features

- **Login screen** — enter a name before joining the chat (`register_menu.py`).
- **Real time** — every message is instantly broadcast to all participants.
- **Multi-pane UI** — animated sidebar, chat log, message input.
- **Change nickname** — right inside the chat, announced to everyone else.
- **Light / dark theme** — switcher in the sidebar.
- **Responsive layout** — widgets are re-placed as the window is resized.
- **Background connection** — incoming messages are read in a separate thread (`threading`).
- **`.exe` build** via PyInstaller (`main.spec`).

## 🕹️ Controls

| Action | How |
|--------|-----|
| Send a message | `Enter` or the `>` button |
| Open / close the sidebar | the `>` / `<` button on the left |
| Change nickname | *Sidebar → Change nick* |
| Change theme | *Sidebar → Dark / Light* |

## 📦 Requirements

- Python **3.10+**
- [customtkinter](https://github.com/tomaskroupa/customtkinter) `>= 5.2`
- [Pillow](https://pillow.readthedocs.io/) (for the images in the forms)
- [PyInstaller](https://pyinstaller.org/) — only if you want to build the `.exe`

Install the dependencies:

```bash
pip install customtkinter pillow pyinstaller
```

## 🚀 Running

### 1. Server

```bash
python server.py
# Сервер запущено на 127.0.0.1:8080
```

### 2. Client

```bash
python main.py
```

Type a name in the login window — and you are in the chat.

> **Important:** by default the client connects to a public ngrok tunnel
> (`7.tcp.eu.ngrok.io:20602`), not to the local server. To test locally, change the
> connection line in `client.py:71`:
> ```python
> self.sock.connect(("127.0.0.1", 8080))
> ```
> To play with friends over the internet — start a tunnel:
> ```bash
> ngrok tcp 8080
> ```
> and put the host and port it gives you into `client.py`.

## 🗂 Project structure

```
tgcopia/
├── main.py            # Entry point: RegisterWindow + mainloop
├── register_menu.py   # Login / registration window (CustomTkinter + Pillow)
├── client.py          # Chat window: socket, send/receive, sidebar, themes
├── server.py          # TCP server: broadcasting messages in threads
├── main.spec          # PyInstaller configuration
├── img/               # bg.png, setting.png — UI assets
└── venv/              # virtual environment (not committed)
```

## 🔌 Protocol

Plain text, line based, lines separated by `\n`:

```
TEXT@<author>@<message>\n
```

Example:

```
TEXT@Yura@Hello, chat!\n
```

The client reads the stream, splits it into lines by `\n` and passes them to
`handle_line()`, which parses them by `@`. Unknown message types are ignored with
a "This message is not supported by your version" notice.

## 📦 Building the `.exe`

```bash
pyinstaller main.spec
```

The result appears in `dist/main.exe`. The `build/`, `dist/`, `__pycache__/` and
`venv/` folders are already listed in `.gitignore`.

## 🗺 Roadmap

- [ ] Remove the hardcoded ngrok host, move settings to `.env` / a config file
- [ ] Real authentication instead of just a name
- [ ] Chat history persistence
- [ ] Rooms and private messages
- [ ] Connection error handling (reconnect)
- [ ] Bundle the assets (`img/`) through PyInstaller
- [ ] Desktop builds for macOS / Linux

## ⚠️ Known limitations

- No authentication — anyone can join under any nickname.
- Traffic is not encrypted (plain `socket`, no TLS).
- No message length limit, client cap is only `listen(5)`.
- Chat history is lost on exit.

## 📄 License

[MIT](LICENSE) © 2026 YuraWoin

---

Made with 🐍 and ☕ on CustomTkinter.
