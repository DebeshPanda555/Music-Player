# Music-Player
import os
import tkinter as tk
from tkinter import filedialog
from pygame import mixer

class MusicPlayer:
    def __init__(self, root):
        self.root = root
        self.root.title("Music Player")
        self.root.geometry("400x200")

        mixer.init()

        self.current_song = None
        self.playing_state = False

        self.play_button = tk.Button(root, text="Play", command=self.play_pause)
        self.play_button.pack(pady=20)

        self.stop_button = tk.Button(root, text="Stop", command=self.stop)
        self.stop_button.pack(pady=20)

        self.next_button = tk.Button(root, text="Next", command=self.next_song)
        self.next_button.pack(pady=20)

        self.prev_button = tk.Button(root, text="Previous", command=self.prev_song)
        self.prev_button.pack(pady=20)

        self.load_button = tk.Button(root, text="Load Songs", command=self.load_songs)
        self.load_button.pack(pady=20)

        self.song_list = []
        self.current_song_index = 0

    def load_songs(self):
        self.song_list = filedialog.askopenfilenames(title="Select Songs", filetypes=[("MP3 Files", "*.mp3")])
        if self.song_list:
            self.current_song_index = 0
            self.current_song = self.song_list[self.current_song_index]

    def play_pause(self):
        if not self.song_list:
            return

        if self.playing_state:
            mixer.music.pause()
            self.play_button.config(text="Play")
        else:
            mixer.music.load(self.current_song)
            mixer.music.play()
            self.play_button.config(text="Pause")
        self.playing_state = not self.playing_state

    def stop(self):
        mixer.music.stop()
        self.playing_state = False
        self.play_button.config(text="Play")

    def next_song(self):
        if not self.song_list:
            return

        self.current_song_index = (self.current_song_index + 1) % len(self.song_list)
        self.current_song = self.song_list[self.current_song_index]
        self.playing_state = False
        self.play_pause()

    def prev_song(self):
        if not self.song_list:
            return

        self.current_song_index = (self.current_song_index - 1) % len(self.song_list)
        self.current_song = self.song_list[self.current_song_index]
        self.playing_state = False
        self.play_pause()

if __name__ == "__main__":
    root = tk.Tk()
    app = MusicPlayer(root)
    root.mainloop()
