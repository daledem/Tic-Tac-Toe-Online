# Tic-Tac-Toe Online

## About

A small Java project built for the purpose of **learning socket programming** and gaining a better understanding of how computer networks work. This is not a production-ready application — it is a hands-on educational exercise that explores TCP sockets, client-server communication, and concurrent connection handling.

## Architecture

The application follows a classic **client-server model** using TCP sockets.

### Server

- **`Servidor.java`** — Main server class. Creates a `ServerSocket` on port `55555` and runs an infinite loop calling `accept()` to wait for new client connections. Each accepted connection is handed off to a thread pool (`ExecutorService` with a cached thread pool) so multiple clients can be handled concurrently without blocking the accept loop.

- **`AdministradorUsuarios.java`** — Extends `Thread` and handles the lifecycle of a single client connection. Reads incoming messages using a `BufferedReader` and writes responses via a `PrintStream`. The protocol is text-based and includes commands such as:
  - `PUT <username> <password>` — Register a new user
  - `GET <username> <password>` — Log in an existing user
  - `CHANGE <username> <elo>` — Update a player's ELO rating
  - `TABLE <port>` — Create a new game table (host a match)
  - `EXIT` — Terminate the connection

  User data is persisted through XML files, demonstrating basic file I/O alongside network communication.

### Client

The client is a Java Swing GUI application. The main entry point (`Cliente.java`) launches the `MenuPrincipal` window, which guides the user through registration, login, and the game lobby.

The client communicates with the server over TCP, sending text commands and reading responses to drive the GUI state.

## Project Structure

```text
Tic-Tac-Toe-Online/
├── pom.xml                          # Maven build configuration
└── src/
    └── main/
        ├── java/
        │   └── org/
        │       ├── cliente/         # Client-side classes (Swing GUI)
        │       │   ├── Cliente.java
        │       │   ├── Lobby.java
        │       │   ├── MenuPrincipal.java
        │       │   ├── Registrarse.java
        │       │   └── ...
        │       ├── servidor/        # Server-side classes
        │       │   ├── Servidor.java
        │       │   └── AdministradorUsuarios.java
        │       └── TresEnRaya/      # Game logic
        └── xml/                     # User data storage (XML)
```

## How to Run

### Prerequisites

- **Java 17** or higher (the `pom.xml` targets Java 17)
- **Maven** for dependency management and building
- **IntelliJ IDEA** is recommended — the Swing GUI forms were designed with the IntelliJ Swing GUI Designer, and running the client from other IDEs may cause layout issues

### Steps

1. **Clone the repository:**

   ```bash
   git clone https://github.com/daledem/Tic-Tac-Toe-Online.git
   cd Tic-Tac-Toe-Online
   ```

2. **Build the project:**

   ```bash
   mvn clean compile
   ```

3. **Start the server:**

   Run the `main` method in `org.servidor.Servidor`. The server will start listening on port `55555`.

4. **Start one or more clients:**

   Run the `main` method in `org.cliente.Cliente`. Repeat this step to open additional client windows.

5. **Play:**

   Register a new account, log in, create or join a game table, and play Tic-Tac-Toe against another connected player.

> **Note:** The GUI was built with the IntelliJ Swing GUI Designer. If you run the client from a different environment (e.g., Eclipse, VS Code), the window layout may not render correctly.

## What You Can Learn

This project is a practical playground for several core networking and concurrency concepts:

- **TCP socket fundamentals** — Creating `ServerSocket` and `Socket` instances, reading and writing streams.
- **Client-server protocol design** — Using simple text-based commands (`PUT`, `GET`, `CHANGE`, `TABLE`, `EXIT`) to exchange structured data.
- **Concurrent connection handling** — Using a thread pool (`ExecutorService`) to handle multiple clients simultaneously without blocking the accept loop.
- **Stream I/O over networks** — Working with `BufferedReader` / `PrintStream` wrapped around socket input/output streams.
- **Basic persistence** — Storing and retrieving user data (usernames, passwords, ELO ratings) in XML files on the server side.
- **Swing GUI + networking** — Integrating a desktop GUI client with a remote server.

## Limitations

This is a learning project, not a polished product. Some known limitations include:

- The protocol is text-based and has no encryption — passwords are sent in plain text.
- The server does not use a database; user data is stored in XML files.
- The GUI relies on IntelliJ’s Swing Designer and may not work correctly in other IDEs.
- Error handling is minimal — the focus is on understanding the mechanics, not on robustness.

## License

This project has no explicit license. It was created as a personal learning exercise.
