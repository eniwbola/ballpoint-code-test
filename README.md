# Ballpoint Code: test repository

A public **test** repository for [Ballpoint Code](https://code.ballpoint.app), the part of Ballpoint that lets a person
approve each commit on their phone and keeps a record of how the code got into the files (typed, pasted, a suggestion, a
tool). The commits here are experiments: expect small, odd changes. This is not the Ballpoint source code.

## What to look at

Most commits carry a line like this in their message:

```
Ballpoint-Proof: https://verify.ballpoint.app/agent/receipt/<id>
```

That commit was approved on a paired phone: the phone showed what was being committed, the person tapped the matching
number and confirmed with Face ID, a fingerprint or the phone's passcode. Three ways to check one:

1. **Open the link.** The receipt page says who approved what (a phone, with which gesture) and that the signatures check out.
2. **Paste the commit on [code.ballpoint.app/code](https://code.ballpoint.app/code)**: either its GitHub link
   (`https://github.com/eniwbola/ballpoint-code-test/commit/<sha>`) or the output of `git cat-file commit <sha>`.
   Then drop a file in the second box to see whether that exact content is in the approved commit.
3. **In a clone**, with [git-ballpoint](https://www.npmjs.com/package/git-ballpoint) installed:
   ```bash
   git clone https://github.com/eniwbola/ballpoint-code-test && cd ballpoint-code-test
   git ballpoint verify HEAD~1      # or any commit with a Ballpoint-Proof line
   ```
   It recomputes the commit's content hash and checks the phone's signature offline.

**Hash-only by default.** The Ballpoint server keeps only hashes and commit ids. The full card the phone showed (file
names, the commit message) is kept in this repository as git notes (`refs/notes/ballpoint-cards`), which `git ballpoint
verify` fetches by itself. To read them yourself: `git fetch origin refs/notes/ballpoint-cards:refs/notes/ballpoint-cards`.

## Use it on your own repository

1. Install git-ballpoint (Node 20 or newer): `npm i -g git-ballpoint`. Phone approvals need version 0.2.0 or newer
   (`git ballpoint --version`); older versions approve with a QR code and a passkey instead.
2. In your repository: `git ballpoint init https://verify.ballpoint.app`
3. Commit as usual. The first time, a pairing code appears: scan it with the **Ballpoint app** (Home → Second device →
   Scan pairing code). From then on each commit goes to your phone: tap the number your computer shows and confirm.
   No app? Choose the QR + passkey route; it proves less (a passkey holder, not an attested phone).
4. Check any commit with `git ballpoint verify <sha>`.

**In VS Code**, the Ballpoint Code extension shows the approval beside your code and marks which lines were typed,
pasted or written by a tool. Install it from [code.ballpoint.app](https://code.ballpoint.app) (Install), then run
**Ballpoint: Pair my phone for approvals** once.

What an approval proves: a person holding that phone approved exactly this commit. It does not say who wrote the code or
whether it is any good.
