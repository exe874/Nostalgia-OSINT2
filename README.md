import streamlit as st
import requests

st.set_page_config(page_title="Nostalgia OSINT", page_icon="🔍")
st.title("🔍 Nostalgia-OSINT")
st.write("Tool OSINT 100% legali - solo info pubbliche")

menu = st.sidebar.selectbox("Scegli tool", ["Username Search", "IP Info", "Domain Info"])

# TOOL 1 - USERNAME
if menu == "Username Search":
    st.header("Cerca Username su Social")
    username = st.text_input("Username da cercare")
    if st.button("Cerca") and username:
        siti = {
            "Instagram": f"https://www.instagram.com/{username}",
            "GitHub": f"https://github.com/{username}",
            "TikTok": f"https://www.tiktok.com/@{username}",
            "Reddit": f"https://www.reddit.com/user/{username}",
            "YouTube": f"https://www.youtube.com/@{username}",
            "Pinterest": f"https://www.pinterest.com/{username}/"
        }
        for nome, link in siti.items():
            try:
                r = requests.get(link, timeout=5, headers={"User-Agent":"Mozilla/5.0"})
                if r.status_code == 200:
                    st.success(f"✅ Trovato su {nome} -> {link}")
                else:
                    st.error(f"❌ Non trovato su {nome}")
            except:
                st.warning(f"⚠️ Errore {nome}")

# TOOL 2 - IP INFO
elif menu == "IP Info":
    st.header("Info su IP pubblico")
    ip = st.text_input("Inserisci IP (es. 8.8.8.8)")
    if st.button("Cerca IP") and ip:
        try:
            r = requests.get(f"https://ipinfo.io/{ip}/json").json()
            st.json(r)
        except:
            st.error("IP non valido")

# TOOL 3 - DOMINIO
elif menu == "Domain Info":
    st.header("Info Dominio")
    dominio = st.text_input("Dominio (es. google.com)")
    if st.button("Cerca Dominio") and dominio:
        st.info(f"Controlla qui per info complete:")
        st.write(f"https://who.is/whois/{dominio}")
        st.write(f"https://urlscan.io/search/#{dominio}")
