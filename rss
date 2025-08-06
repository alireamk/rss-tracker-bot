import streamlit as st
import feedparser
from datetime import datetime

rss_feeds = [
    "https://www.feedspot.com/infiniterss.php?_src=followbtn&followfeedid=30452&q=site:https%3A%2F%2Fwww.adweek.com%2Ffeed%2F",
    "https://feeds.feedburner.com/ad-exchange-news",
    "https://blog.storeya.com/feed/"
]

keywords = ["advertising", "تبلیغات"]

def fetch_articles(rss_urls, keywords):
    results = []
    for url in rss_urls:
        feed = feedparser.parse(url)
        for entry in feed.entries:
            title = entry.title
            summary = entry.get("summary", "")
            content = title + " " + summary

            for kw in keywords:
                if (kw.lower() in content.lower()) if kw.isascii() else (kw in content):
                    results.append({
                        "title": title,
                        "link": entry.link,
                        "summary": summary,
                        "matched_keyword": kw
                    })
                    break
    return results

st.set_page_config(page_title="RSS Keyword Tracker", layout="wide")
st.title("📡 RSS Article Tracker")
st.markdown("Finds articles containing keywords like **advertising / تبلیغات**.")

if st.button("🔄 Refresh Now"):
    st.session_state["articles"] = fetch_articles(rss_feeds, keywords)
    st.session_state["last_checked"] = datetime.now()

if "articles" not in st.session_state:
    st.session_state["articles"] = fetch_articles(rss_feeds, keywords)
    st.session_state["last_checked"] = datetime.now()

st.write(f"🕒 Last checked: {st.session_state['last_checked'].strftime('%Y-%m-%d %H:%M:%S')}")
for a in st.session_state["articles"]:
    st.markdown(f"""
    **{a['title']}**  
    _Keyword:_ `{a['matched_keyword']}`  
    🔗 [Read More]({a['link']})  
    {a['summary'][:300]}...
    ---
    """)
