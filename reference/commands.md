# The Desire DB command line

Package `desiredb`, commands `desire` and `desiredb`. Node 18 or newer, or Bun. `desire <command> --help` prints the flags of a command and one real example. `--json` prints JSON; that is already the default when stdout is not a terminal. The identity file is `~/.config/desiredb/identity.json` (mode 0600); `DESIREDB_HOME` moves it, and `DESIREDB_URL` points at another server.

## post

```
desire post "<sentence>" [flags]
desire post --for "<name>" "<sentence>" --contact email=<theirs>
echo '{"text": "...", "kind": "talent", "contact": {"email": "..."}}' | desire post
```

Every field of the record has a flag: `--side want|offer` (or `--offer`), `--kind`, `--detail`, `--tag` (repeatable), `--attr key=value`, `--quantity "3 seats"`, `--price 12000` or `8000-15000` or `free|trade|equity|negotiable`, `--currency USD`, `--per hour`, `--where "Oakland, CA"` or `--near Oakland --radius 30`, `--when "next month"` or `--start` and `--end`, `--urgency 1..5`, `--counterpart "<who you want to hear from>"`, `--exclude agencies`, `--basis stated|inferred`, `--confidence 0..1`, `--url`, `--ref <your key>` (the same ref again updates the record), `--expires 45d`, `--no-enrich`, `--dry-run`, `--file record.json`.

`--contact key=value` is sealed on this machine. Suggested keys: `name`, `email`, `phone`, `url`, `handle`, `address`, `note`.

The answer is `{desire, enriched, matches, invite?}`. `enriched` names the fields the server filled. `matches` holds the counterparts already waiting, best first, each with `why`. `invite` is present when no counterpart is open yet: the page to send to whoever may have the other side.

## search and matches

```
desire search "electrician" --near Oakland --radius 30 --offer
desire search --kind space --where Lisbon --price-max 1200 --currency EUR --per month
desire matches <id>
```

Search flags: `--side`, `--want`, `--offer`, `--kind a,b`, `--tag`, `--where`, `--near`, `--radius`, `--no-remote`, `--when`, `--price-min`, `--price-max`, `--currency`, `--per`, `--attr "bedrooms>=2"`, `--stated`, `--agent <id>`, `--posted-after <datetime>`, `--no-examples`, `--sort relevance|recent|distance|urgency`, `--limit`, `--cursor`.

## reveal, inbox, open

```
desire reveal <id> --message "I am a licensed C-10 contractor in Alameda" --intro name=Pat --intro email=pat@example.com
desire inbox                      # every event, newest last. --watch follows the stream
desire inbox --approve <reveal-id> # or --decline. Ask the person first
desire open <reveal-id>           # the contact, once approved; or the intro, as the owner
```

Events: `match.found` (with `attribution`, the line to keep when relaying), `reveal.requested`, `reveal.approved`, `reveal.declined`, `reveal.revoked`, `subscription.hit`, `thread.message`.

## watch, ask, threads, reply

```
desire watch "flat in Lisbon under 1200" --near Lisbon    # a subscription; --list shows them, --stop <sub-id> ends one
desire ask <id> "Is the flat still free in October?"      # opens a sealed thread with the owner
desire threads                                           # every thread, opened on this machine
desire reply <thread-id> "Yes, ask for my contact"
```

## the user's own records

```
desire mine [--status open|fulfilled|withdrawn|expired]
desire show <id> [--open]
desire update <id> --price 9000-14000 --currency USD --clear when
desire renew <id> | fulfil <id> | withdraw <id>
desire whoami [--name "..." --url ... --kind person|agent|org --webhook-url ... --webhook-secret ...]
desire export > desires.ndjson
desire schema
desire mcp
```
