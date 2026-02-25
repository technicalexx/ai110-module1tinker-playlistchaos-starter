# Playlist Chaos

---

Issues:

1. Song with energy level below 7 appears under Hype Section -> check logic
2. Adding the same song and artist but changing energy levels/genre adds all options of the same song into different playlists -> should be addressed
3. Using the "Feeling Lucky" function sometimes provides the same song 3-4 times in a row -> could be fine since it's supposed to be randomized. However, we need to check the logic to make sure there are no apparent issues with the logic.
4. Adding the same with the exact same characteristics (by clicking "Add to playlist" multiple times) keeps adding the exact same record to the playlist -> needs to be fixed
5. Changing "Hype min energy" level to 9" -> includes a few songs with lower level of energy (same change in "Chill max energy" works properly) -> logic issue
6. Search issues: no matches when entering only part of the name or using lower cases for song names

Attempted Solutions:

1. and 5. Could be intended behavior because it's not only checking energy level but also checking certain genre
2. and 4. Fixed the possibility to add duplicates or same song + same artist with different genre
3. Since it is randomized, most likely getting the same song 3-4 times in a row is appropriate. However, the logic itself has 2 bugs:
   - random_choice_or_none() will throw if songs is empty (it never returns None)
   - “any” mode excludes Mixed (your spec says ideally include Mixed)
     => lucky_pick() and random_choice_or_none() functions were adjusted
4. Backwards search -> fixed + added partial match

Additional small bugs based on the code itself:

1. In compute_playlist_stats():
   - total = len(hype) (wrong — should be total unique songs)
   - avg_energy sums only hype energy but divides by all_songs
   - hype_ratio uses wrong denominator

-> logic was adjusted + made stats use unique songs

2. Chill keyword check is case-sensitive right now. If the title contains “LoFi” or “Ambient” with different casing, it can miss.

Additional suggestion from AI:

merge_playlists mutates lists
This isn’t causing any main issues right now, but it’s a classic “AI wrote it wrong” landmine:
merged[key] points at the same list as a[key], so .extend() mutates a too.

---

Your AI assistant tried to build a smart playlist generator. The app runs, but some of the behavior is unpredictable. Your task is to explore the app, investigate the code, and use an AI assistant to debug and improve it.

This activity is your first chance to practice AI-assisted debugging on a codebase that is slightly messy, slightly mysterious, and intentionally imperfect.

You do not need to understand everything at once. Approach the app as a curious investigator, work with an AI assistant to explain what you find, and make targeted improvements.

---

## How the code is organized

### `app.py`

The Streamlit user interface. It handles things like:

- Showing and updating the mood profile
- Adding songs
- Displaying playlists
- Lucky pick
- Stats and history

### `playlist_logic.py`

The logic behind the app, including:

- Normalizing and classifying songs
- Building playlists
- Merging playlist data
- Searching
- Computing statistics
- Lucky pick mechanics

You will need to look at both files to understand how the app behaves.

---

## What you will do

### 1. Explore the app

Run the app and try things out:

- Add several songs with different titles, artists, genres, and energy levels
- Change the mood profile
- Use the search box
- Try the lucky pick
- Inspect the playlist tabs and stats
- Look at the history

As you explore, write down at least five things that feel confusing, inconsistent, or strange. These might be bugs, quirks, or unexpected design decisions.

### 2. Ask AI for help understanding the code

Pick one issue from your list. Use an AI coding assistant to:

- Explain the relevant code sections
- Walk through what the code is supposed to do
- Suggest reasons the behavior might not match expectations

For example:

> "Here is the function that classifies songs. The app is mislabeling some songs. Help me understand what the function is doing and where the logic might need adjustment."

Before making changes, summarize in your own words what you think is happening.

### 3. Fix at least four issues

Make improvements based on your investigation.

For each fix:

- Identify the source of the issue
- Decide whether to accept or adjust the AI assistant's suggestions
- Update the code
- Add a short comment describing the fix

Your fixes may involve logic, calculations, search behavior, playlist grouping, lucky pick behavior, or anything else you discover.

### 4. Test your changes

After each fix, try interacting with the app again:

- Add new songs
- Change the profile
- Try search and stats
- Check whether playlists behave more consistently

Confirm that the behavior matches your expectations.

### 5. Optional stretch goals

If you finish early or want an extra challenge, try one of these:

- Improve search behavior
- Add a "Recently added" view
- Add sorting controls
- Improve how Mixed songs are handled
- Add new features to the history view
- Introduce better error handling for empty playlists
- Add a new playlist category of your own design

---

## Tips for success

- You do not need to solve everything. Focus on exploring and learning.
- When confused, ask an AI assistant to explain the code or summarize behavior.
- Test the app often. Small experiments reveal useful clues.
- Treat surprising behavior as something worth investigating.
- Stay curious. The unpredictability is intentional and part of the experience.

When you finish, Playlist Chaos will feel more predictable, and you will have taken your first steps into AI-assisted debugging.
