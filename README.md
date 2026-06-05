# Blind Valet CSV Exporter

A Chrome extension that exports tournament data from the Blind Valet web app into a CSV file.

This project is intended for players, hosts, and club admins who already have access to tournament data in Blind Valet and want a portable CSV export that can be reviewed, cleaned up, or imported into another system.

## What It Does

Blind Valet CSV Exporter runs inside the Blind Valet website and crawls the rendered tournament pages that your Blind Valet account can already see. It collects visible tournament data and generates a downloadable CSV.

The exporter currently reads:

- Tournament list data from the lobby
- Tournament name, date, and time
- Buy-in, re-entry cost, add-on cost, and prize pool values when visible
- Player names
- Player finishing positions
- Player prize values from finished tournament result rows
- Rebuy and add-on markers shown beside player names
- Knockout counts and visible knocked-out-by names when Blind Valet shows them
- Payout table data when available

The extension does not bypass Blind Valet permissions. It only exports data visible to the signed-in Blind Valet user.

## Important Notes

- Blind Valet can reuse the `Stack` column for final prize values after a tournament is finished. The exporter treats those finished-tournament values as `prize` and leaves `stack` blank in the CSV.
- Blind Valet can show base payout table values that differ from final player-row prize values when bounties are involved. The exporter trusts the player result rows when they show final prize values.
- Blind Valet allows player names that differ only by letter casing. Since many import systems treat names case-insensitively, the exporter preserves the first spelling and adds numeric suffixes to later case-only duplicates in the CSV. For example, `johnny`, `joHnny`, and `Johnny` become `johnny`, `joHnny1`, and `Johnny2`. This keeps them importable as separate people, and you can rename or merge them later in the destination app.
- The extension depends on Blind Valet's rendered HTML. If Blind Valet changes its UI structure, the exporter may need updates.

## Install for Local Use

1. Download or clone this repository.
2. Open Chrome.
3. Go to `chrome://extensions`.
4. Enable `Developer mode`.
5. Click `Load unpacked`.
6. Select the extension folder.

If this folder is still inside a larger project, select:

```text
browser-extensions/blind-valet-csv-exporter
```

If this project has been moved to its own repository, select the repository root.

## How to Use

1. Log in to Blind Valet.
2. Open `https://blindvalet.com` or `https://app.blindvalet.com`.
3. Open the Blind Valet lobby or any tournament page.
4. Look for the exporter banner injected near the top of the page.
5. Click `IMPORT`.
6. Leave `Export only finished tournaments` enabled if you only want completed tournament results.
7. Set the tournament date range. `From` defaults to `2020-01-01` and is required. `To` defaults to today; leave it empty to export through today.
8. Click `Start export`.
9. Wait for the exporter to crawl the visible tournament pages in the selected range.
10. Click `Download CSV`.
11. Review the CSV before importing it anywhere else.

## CSV Output

The generated CSV is a flat tournament import file. Each row represents a player entry in a tournament.

Current columns include:

```text
tournament_key,name,start_time,buy_in_cost,starting_stack,places_paid,rebuy_cost,add_on_cost,bounty_tournament,bounty_per_player,total_prize_override,player_name,position,status,rebuy_count,addon_count,prize,stack,knocked_out_by_name,member_key,member_name
```

The exact schema may change as Blind Valet's UI changes or as the exporter improves.


## Privacy

This extension does not send tournament data to an external server. It reads data from the current Blind Valet tab and writes a CSV file in the browser.

Review the source code before using it with sensitive tournament or member data.


## License

MIT. See [LICENSE](LICENSE).

You are free to use, fork, modify, and redistribute this project under the terms of the MIT License.

## Disclaimer

This project is not affiliated with, endorsed by, or sponsored by Blind Valet.

Use it only with Blind Valet accounts and tournament data you are authorized to access.
