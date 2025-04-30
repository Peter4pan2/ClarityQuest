

# ClarityQuest - A Game Smart Contract

A decentralized ClarityQuest game smart contract built in Clarity for the Stacks blockchain. This contract enables users to create trivia games, participate by submitting answers with a stake, and distribute the prize pool to the winner(s) in a secure, transparent, and permissionless manner.

---

## 🚀 Features

- **Create Trivia Games**: Users can create a new trivia game by providing a question, the correct answer, and an initial STX stake to seed the prize pool.
- **Participate in Games**: Players can submit answers with a stake to contribute to the prize pool.
- **Finalize Games**: The creator of a trivia game can finalize the game and reward the winner.
- **Submission Validation**: Ensures only one answer per participant and validates the correctness of data.
- **Emergency Shutdown**: Admins can shut down active games in emergencies and refund the game creator.
- **Ownership Transfer**: Contract admin role can be securely transferred.

---

## 📦 Contract Components

### Constants

| Constant | Description |
|---------|-------------|
| `ERR-UNAUTHORIZED-ACCESS` | Caller is not authorized. |
| `ERR-TRIVIA-GAME-EXISTS` | Trivia game with this ID already exists. |
| `ERR-TRIVIA-GAME-NOT-FOUND` | Specified trivia game doesn't exist. |
| `ERR-INVALID-STAKE-AMOUNT` | Stake provided is invalid (e.g., negative). |
| `ERR-TRIVIA-GAME-ENDED` | Game is no longer active. |
| `ERR-DUPLICATE-ANSWER` | Participant has already submitted an answer. |
| `ERR-INSUFFICIENT-STX-BALANCE` | Transfer failed due to low balance. |
| `ERR-INVALID-INPUT` | Question or answer doesn't meet validation requirements. |

---

## 🧾 Data Structures

### `trivia-game-records` (Map)

Holds metadata and status for each trivia game.

- `game-creator`: `principal`
- `trivia-question`: `string-utf8`
- `correct-answer`: `string-utf8`
- `total-prize-pool`: `uint`
- `game-active-status`: `bool`
- `winning-player`: `optional principal`

### `participant-submission-records` (Map)

Tracks participant answers.

- Key: `{ trivia-game-id, participant-address }`
- Value: `{ submitted-answer, submission-timestamp }`

---

## 🔍 Read-Only Functions

- `get-trivia-game-information (trivia-game-id)`: Retrieves full game data.
- `get-participant-submission (trivia-game-id, participant-address)`: Retrieves a participant’s answer.
- `get-contract-administrator`: Returns current admin.

---

## 🛠️ Public Functions

### Game Management

- `create-trivia-game (question, answer, initial-stake)`: Creates a new trivia game and seeds it with STX.
- `submit-participant-answer (game-id, answer, stake)`: Allows users to answer a trivia question by staking STX.
- `finalize-trivia-game (game-id)`: Ends the game and transfers the prize pool to the winner or creator if no correct answer exists.

### Administration

- `transfer-contract-ownership (new-admin)`: Admin can assign contract ownership to a new principal.

### Emergency Controls

- `emergency-game-shutdown (game-id)`: Admin-only function to end a game and refund the creator.

---

## ✅ Validation Logic

- String inputs must be between 1 and 256 characters.
- Only one submission per player per game.
- Games must be active to accept answers or be finalized.
- All STX transfers are checked for balance sufficiency.

---

## 📜 Example Workflow

1. **Create a Game**:
   ```clojure
   (create-trivia-game "What is 2+2?" "4" u1000000)
   ```

2. **Submit Answer**:
   ```clojure
   (submit-participant-answer u0 "4" u500000)
   ```

3. **Finalize Game**:
   ```clojure
   (finalize-trivia-game u0)
   ```

---

## 🔒 Security Considerations

- Admin-only functions are gated by strict checks.
- All critical operations like STX transfers are wrapped in assertions.
- No duplicate answers allowed per user/game combo.

---

## 📄 License

This smart contract is released under the MIT License.

