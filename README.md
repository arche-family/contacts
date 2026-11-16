# Contacts

The Arche phone's contacts, written in [Arche](https://github.com/arche-family/arche-lang): a WebAssembly component of the Arche phone's world, built into the phone as one of its default apps and updated from its repository. That world is Nimadir's mini-app world and what a phone adds to it; its WIT is in `wit/` (from arche-os's `phone/wit`), and `world` names it.

It works like Android's:

- The phone's contacts, a name for each number, in order of their names. The phone keeps them, not the app: Phone and Messages show the names too, and so do the call screen and the notifications of messages that come.
- A contact opens to Call, which the phone's call screen takes from there, and Delete.
- A new contact is its name and number, and Save, which empties the form for the next; a number saved again is renamed.

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
