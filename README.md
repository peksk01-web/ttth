
import tkinter as tk
from tkinter import ttk
from datetime import datetime


class WaryongHelper:
    def __init__(self, root):
        self.root = root
        self.root.title("Waryong2 Yardımcı")
        self.root.geometry("460x430")
        self.root.resizable(False, False)

        self.running = False
        self.mode = tk.StringVar(value="Vurma")
        self.status = tk.StringVar(value="Hazır")

        ttk.Label(
            root,
            text="WARYONG2 YARDIMCI",
            font=("Segoe UI", 17, "bold")
        ).pack(pady=(18, 4))

        ttk.Label(
            root,
            text="Modunu seç ve Başlat düğmesine bas."
        ).pack(pady=4)

        modes = ttk.LabelFrame(root, text="Çalışma modu", padding=12)
        modes.pack(fill="x", padx=20, pady=12)

        ttk.Radiobutton(
            modes,
            text="Otomatik Vurma",
            variable=self.mode,
            value="Vurma"
        ).pack(anchor="w", pady=5)

        ttk.Radiobutton(
            modes,
            text="Otomatik Çekme",
            variable=self.mode,
            value="Çekme"
        ).pack(anchor="w", pady=5)

        ttk.Label(
            root, textvariable=self.status,
            font=("Segoe UI", 11, "bold")
        ).pack(pady=6)

        buttons = ttk.Frame(root)
        buttons.pack(pady=8)

        ttk.Button(
            buttons, text="Başlat",
            command=self.start
        ).pack(side="left", padx=5)

        ttk.Button(
            buttons, text="Durdur",
            command=self.stop
        ).pack(side="left", padx=5)

        ttk.Button(
            buttons, text="Günlüğü Temizle",
            command=self.clear
        ).pack(side="left", padx=5)

        self.log = tk.Text(root, height=9, width=52)
        self.log.pack(padx=15, pady=10)
        self.log.configure(state="disabled")

        self.write_log("Program açıldı. Hazır.")

        self.root.protocol("WM_DELETE_WINDOW", self.close)

    def write_log(self, message):
        timestamp = datetime.now().strftime("%H:%M:%S")
        self.log.configure(state="normal")
        self.log.insert("end", f"[{timestamp}] {message}\n")
        self.log.see("end")
        self.log.configure(state="disabled")

    def start(self):
        if self.running:
            return

        self.running = True
        selected = self.mode.get()
        self.status.set(f"Durum: {selected} modu seçildi")
        self.write_log(
            f"{selected} modu başlatıldı. "
            "Arayüz test modunda; oyuna komut gönderilmiyor."
        )

    def stop(self):
        if self.running:
            self.write_log("İşlem durduruldu.")

        self.running = False
        self.status.set("Hazır")

    def clear(self):
        self.log.configure(state="normal")
        self.log.delete("1.0", "end")
        self.log.configure(state="disabled")

    def close(self):
        self.stop()
        self.root.destroy()


if __name__ == "__main__":
    root = tk.Tk()
    app = WaryongHelper(root)
    root.mainloop()
