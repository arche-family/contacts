# Contacts

The Arche phone's contacts, written in [Arche](https://github.com/arche-family/arche-lang): a WebAssembly component of the Arche phone's world, built into the phone as one of its default apps and updated from its repository. That world is Nimadir's mini-app world and what a phone adds to it; its WIT is in `wit/` (from arche-os's `phone/wit`), and `world` names it.

It works like Android's:

- The phone's contacts, a name for each number, in order of their names. The phone keeps them, not the app: Phone and Messages show the names too, and so do the call screen and the notifications of messages that come.
- A contact opens as HyperOS's does: its picture and name, its number on a card with Call and Message by it (Message opens Messages to write to it, `messages.component.wasm?to=<number>`, the phone's intents), the calls with it (`telephony.history`), and Edit and Delete at the top; Delete asks first.
- A new contact, or one edited, is its name and its number, and Save (✓). The number's field asks for the phone's dial pad (`type="tel"`); only a phone number is saved (3 to 15 digits, a + before them, spaces, hyphens and brackets dropped), and what is no number is said under it. A number changed in an edit takes the name from the old one.

It shows in the phone's language, English, Uzbek or Russian, as the phone's `locale` says (`en-US`, `uz-UZ`, `ru-RU`): its words, the count of contacts with Russian's plural forms, and the calls' dates and lengths; it makes its page again when the language changes.

`manifest` says what it may do: read and change the contacts (`contacts`) and make calls (`telephony`).

## Build

With [arche-lang](https://github.com/arche-family/arche-lang) checked out next to this repository and built (`make -C stage0 && make -C compiler && make -C wasm`):

```sh
../arche-lang/wasm/nimadir-app.sh . contacts.component.wasm
```

## Test

`headless` holds a headless run's taps and typing (the round + opens a new contact, a name and a number typed, Save; + again, and the form closed) and the functions the phone's world adds, and `expected.txt` what arche-wasm's interpreter said to them, as Nimadir's shell does not run the phone's world. `app-check.sh` runs the app with arche-wasm, by the interpreter and by the JIT, and compares what it says with `expected.txt`; CI runs it when started by hand (Actions' Run workflow). The contacts on the phone itself, the call screen saying a contact's name, are tried by arche-os's `tests/phone.sh`.

```sh
../arche-lang/wasm/app-check.sh .
NIMADIR=~/nimadir ../arche-lang/wasm/app-expected.sh .   # expected.txt again, from arche-wasm
```
