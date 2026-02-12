[app.py](https://github.com/user-attachments/files/25265269/app.py)
import streamlit as st
from datetime import datetime
import random
# Sayfa Ayarları
st.set_page_config(page_title="Ahmet YKS Sistemi", layout="wide")
# --- YKS GERİ SAYIM (Hata Almamak İçin Güvenli Hesaplama) ---
try:
   hedef_tarih = datetime(2026, 6, 14, 10, 0, 0)
   simdi = datetime.now()
   fark = hedef_tarih - simdi
   kalan_gun = int(fark.days)
except Exception:
   kalan_gun = "Hesaplanamadı"
# --- MOTİVASYONLAR ---
motivasyonlar = [
   "Allah sabredenlerle beraberdir. Gayret et Ahmet! ✨",
   "Ailen senin en büyük destekçin, duaları seninle. ❤️",
   "Peygamberimiz (s.a.v): 'İlim öğrenmek her Müslümana farzdır.' 🌙",
   "Sen elinden geleni yap, gerisini Allah'a bırak. 🙏",
   "Zorluklar seni yıldırmasın, zafer yakındır Ahmet. 🏆",
   "Bugün attığın her adım, seni hayaline bir gün daha yaklaştırır."
]
# --- TASARIM (Resimsiz, En Sağlam Versiyon) ---
st.markdown("""
<style>
   .stApp {
       background-color: #003366; /* Deniz Laciverti */
   }
   .card {
       background: white;
       padding: 20px;
       border-radius: 15px;
       color: black;
       box-shadow: 0 4px 15px rgba(0,0,0,0.5);
       margin-bottom: 15px;
   }
   .banner {
       background: #FFD700;
       color: #003366;
       padding: 15px;
       border-radius: 10px;
       text-align: center;
       font-weight: bold;
       margin-bottom: 20px;
   }
   h1 { color: white !important; text-align: center; }
   .stButton>button {
       background: #0077be;
       color: white;
       font-weight: bold;
       width: 100%;
       height: 45px;
   }
</style>
   """, unsafe_allow_html=True)
# --- OTURUM ---
if 'auth' not in st.session_state:
   st.session_state['auth'] = False
if 'liste' not in st.session_state:
   st.session_state['liste'] = {g: [] for g in ["Pazartesi", "Salı", "Çarşamba", "Perşembe", "Cuma", "Cumartesi", "Pazar"]}
# --- GİRİŞ ---
if not st.session_state['auth']:
   _, col2, _ = st.columns([0.1, 1, 0.1])
   with col2:
       st.markdown("<br><br><div class='card' style='text-align:center;'>", unsafe_allow_html=True)
       st.markdown("<h2>⚓ AHMET YKS SİSTEMİ</h2>", unsafe_allow_html=True)
       user = st.text_input("Kullanıcı Adı")
       pw = st.text_input("Şifre", type="password")
       if st.button("GİRİŞ YAP"):
           if user.lower() == "ahmet" and pw == "yks2026":
               st.session_state['auth'] = True
               st.rerun()
           else:
               st.error("Hatalı Giriş!")
       st.markdown("</div>", unsafe_allow_html=True)
# --- ANA EKRAN ---
else:
   # Selamlama
   hr = datetime.now().hour
   if 9 <= hr < 11: s = "🌅 Günaydın Ahmet!"
   elif 11 <= hr < 16: s = "🌞 İyi Günler Ahmet!"
   else: s = "🌃 İyi Akşamlar Ahmet!"
   st.markdown(f"<h1>{s}</h1>", unsafe_allow_html=True)
   st.markdown(f"<div class='banner'>⏳ YKS 2026'ya {kalan_gun} GÜN KALDI<br><small>✨ {random.choice(motivasyonlar)}</small></div>", unsafe_allow_html=True)
   tab1, tab2 = st.tabs(["➕ Konu Ekle", "📅 Günlük Plan"])
   with tab1:
       st.markdown("<div class='card'>", unsafe_allow_html=True)
       ders = st.selectbox("Ders", ["Matematik", "Türkçe", "Fizik", "Kimya", "Biyoloji", "Sosyal"])
       konu = st.text_input("Konu Adı")
       gun = st.selectbox("Hangi Gün?", list(st.session_state['liste'].keys()))
       if st.button("EKLE"):
           if konu:
               st.session_state['liste'][gun].append({"ders": ders, "konu": konu, "tamam": False})
               st.rerun()
       st.markdown("</div>", unsafe_allow_html=True)
   with tab2:
       st.markdown("<div class='card'>", unsafe_allow_html=True)
       secili_gun = st.selectbox("Günü Seç", list(st.session_state['liste'].keys()))
       for i, m in enumerate(st.session_state['liste'][secili_gun]):
           c1, c2 = st.columns([0.2, 0.8])
           with c1:
               st.session_state['liste'][secili_gun][i]['tamam'] = st.checkbox("", value=m['tamam'], key=f"{secili_gun}_{i}")
           with c2:
               if m['tamam']:
                   st.markdown(f"~~{m['ders']} - {m['konu']}~~ ✅")
               else:
                   st.markdown(f"**{m['ders']}** - {m['konu']}")
       if st.button("Bitenleri Listeden Çıkar"):
           for g in st.session_state['liste']:
               st.session_state['liste'][g] = [x for x in st.session_state['liste'][g] if not x['tamam']]
           st.rerun()
       st.markdown("</div>", unsafe_allow_html=True)
   with st.sidebar:
       if st.button("Çıkış Yap"):
           st.session_state['auth'] = False
           st.rerun()
