# On Hammers and Nails

> *"it is tempting, if the only tool you have is a hammer, to treat everything as if it were a nail."* - Abraham Maslow

I had a small problem. I recently went through some bookmarks I had made last year only to discover a large portion of them now 404. For some of the sites this wasn't a huge issue as I could simply just use the [Wayback Machine](http://web.archive.org/), but there were a few that don't seem to have been backed up anywhere. This is a shame.

What I needed was something that could:

* Keep a list of webpages I want to bookmark.
* Create a snapshot of the webpage when I bookmark it.
* Serve the webpage archive back to me.

Being the professional webshit that I am, I immediately started solutionising.

* I need to build a web application.
* The web application needs to be interactive and easy to extend, so with my experience in React it felt like an obvious choice.
* I need a database to store bookmark information, and given I've used Postgres in the past why would I choose something else?
* Deploying it could be a little awkward so I need to containerise the project using Docker. 
* I need to host this somewhere so I could use my Raspberry Pi.

Luckily before I went down any of these ill informed rabbit holes I realised I was doing was stupid. It was as clear as day. 

*"This could be a shell script."*

And so it was.

```
#!/bin/bash

# MIT License
# 
# Copyright (c) [2025] [ChurchOfTuring]
# 
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
# 
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
# 
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

BOOKMARK_DIR="$HOME/bookmarks"
BOOKMARK_FILE="$BOOKMARK_DIR/bookmarks.txt"
PAGES_DIR="$BOOKMARK_DIR/pages"

setup() {
  mkdir -p "$BOOKMARK_DIR"
  mkdir -p "$PAGES_DIR"
  touch "$BOOKMARK_FILE"
}

show_help() {
  echo "bookmark.sh - A simple bookmark archive manager."
  echo ""
  echo "Usage:"
  echo "  ./bookmark.sh add URL [NAME]      - Add a bookmark with optional custom name."
  echo "  ./bookmark.sh list                - List all bookmarks."
  echo "  ./bookmark.sh remove NAME         - Remove a bookmark by name."
  echo "  ./bookmark.sh clean               - Remove all downloaded files and bookmarks."
  echo "  ./bookmark.sh help                - Show this help message."
  echo "  ./bookmark.sh sanity              - Checks if you have the required dependencies."
  echo "  ./bookmark.sh serve [PORT|8080]   - Serve the saved pages with an optional port."
  echo ""
  echo "All bookmarks and pages are relative to \$BOOKMARK_DIR ($BOOKMARK_DIR)."
  echo "Feel free to change \$BOOKMARK_DIR."
}

add_bookmark() {
  local url="$1"
  local name="$2"
  
  # If no name is provided, extract it from the URL.
  if [ -z "$name" ]; then
    name=$(echo "$url" | sed -E 's/https?:\/\/(www\.)?//g' | sed -E 's/\/.*//g')
  fi
  
  # Check if name already exists.
  if grep -q "^$name|" "$BOOKMARK_FILE"; then
      echo "Error: Bookmark with name '$name' already exists."
      return 1
  fi
  
  echo "$name|$url" >> "$BOOKMARK_FILE"
  
  local bookmark_page_dir="$PAGES_DIR/$name"
  mkdir -p "$bookmark_page_dir"
  
  echo "Downloading [$url] to $bookmark_page_dir."

  wget --no-verbose \
    --recursive \
    --convert-links \
    --page-requisites \
    --html-extension \
    --no-host-directories \
    --restrict-file-names=windows \
    --no-parent \
    -P "$bookmark_page_dir" \
    "$url"
    
  echo "Bookmark '$name' added successfully!"
}

list_bookmarks() {
  if [ ! -s "$BOOKMARK_FILE" ]; then
    echo "No bookmarks found."
    return
  fi

  while IFS="|" read -r name url; do
    echo "[$name] - $url"
  done < "$BOOKMARK_FILE"
}

remove_bookmark() {
  local name="$1"
  
  if ! grep -q "^$name|" "$BOOKMARK_FILE"; then
    echo "Error: Bookmark '$name' not found."
    return 1
  fi
  
  # Remove from bookmarks file
  grep -v "^$name|" "$BOOKMARK_FILE" > "$BOOKMARK_FILE.tmp"
  mv "$BOOKMARK_FILE.tmp" "$BOOKMARK_FILE"
  
  # Remove downloaded files
  if [ -d "$PAGES_DIR/$name" ]; then
    rm -rf "$PAGES_DIR/$name"
    echo "Removed files for '$name'."
  fi
  
  echo "Bookmark '$name' removed successfully."
}

# Clears the bookmark file and pages directory.
clean_files() {
  read -p "This will remove all downloaded pages and bookmarks. Continue? (y/n): " confirm
  
  if [ "$confirm" != "y" ]; then
    echo "Operation cancelled."
    return
  fi
  
  rm -rf "$PAGES_DIR"/*
  rm "$BOOKMARK_FILE"
  mkdir -p "$PAGES_DIR"
  
  echo "All downloaded files and bookmarks have been removed."
}

# Serve the pages directory using python.
serve_directory() {
  local port="${1:-8080}"
  python3 -m http.server $port --d $PAGES_DIR
}

# Checks we have wget and python installed. 
sanity_check() {
  local missing=()

  if ! command -v wget &>/dev/null; then
    missing+=("wget")
  fi

  if ! command -v python3 &>/dev/null; then
    missing+=("python3")
  fi

  if [ ${#missing[@]} -eq 0 ]; then
    echo "All dependencies are installed."
    return 0
  else
    echo "Missing dependencies: ${missing[*]}."
    return 1
  fi
}

# Main function that handles dispatching subcommands.
main() {
  setup
  
  local command="$1"
  shift
  
  case "$command" in
    add)
      if [ -z "$1" ]; then
        echo "Error: URL is required."
        echo ""
        show_help
        exit 1
      fi
      add_bookmark "$1" "$2"
      ;;
    list)
      list_bookmarks
      ;;
    remove)
      if [ -z "$1" ]; then
        echo "Error: Bookmark name is required."
        echo ""
        show_help
        exit 1
      fi
      remove_bookmark "$1"
      ;;
    clean)
      clean_files
      ;;
    serve)
      serve_directory "$1"
      ;;
    sanity)
      sanity_check
      ;;
    help|"")
      show_help
      ;;
    *)
      echo "Error: Unknown command '$command'."
      echo ""
      show_help
      exit 1
      ;;
  esac
}

main "$@"
```

* It creates a text file that stores the list of bookmarks.
* Pages can be bookmarked and deleted. 
* When a bookmark is added, wget is used to store a snapshot of the page.
* Using `bookmark.sh serve 9000` we can serve the directory of bookmarked pages on localhost:9000.

# Conclusion

I spend all day writing web services, so it felt like building a fully fledged web application was a very natural solution to the problem at hand. This was a fallacy. The amount of complexity in the initial solution was magnitudes higher than the problem ever justified, and I had fell into the psychological trap that Maslow had articulated nearly 60 years ago. 

I'm not kicking myself over it, but it's a good reminder to take stock of your mental hammers. It also highlights how important it is to have a good repertoire of tools at hand as it's your tools that ultimately define your approach to solving problems. 