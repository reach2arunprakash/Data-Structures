# Jukebox

```java
public class Jukebox {
	private CDPlayer cdPlayer;
	private User user;
	private Set<CD> cdCollection;
	private SongSelector ts;

	public Jukebox(CDPlayer cdPlayer, User user, Set<CD> cdCollection, SongSelector ts) {}
	}

	public Song getCurrentSong(){
		return ts.getCurrentSong();
	}

	public void setUser(User u) {
		this.user = u;
	}
}

public class CDPlayer {
	private Playlist p;
	private CD c;

	public CDPlayer(CD c, Playlist p) {}
	public CDPlayer(Playlist p) { this.p = p;}
	public CDPlayer(CD c) {this.c = c;}

	public Playlist getPlaylist() {
		return p;
	}
	public void setPlaylist(Playlist p) {this.p = p;}

	public CD getCD() {return c;}
	public void setCD(CD c) {
		this.c = c;
	}
}

public class Playlist {
	private Song song;
	private Queue<Song> queue;
	public Playlist(Song song, Queue<Song> queue) {}

	public Song getNextToPlay() {
		return queue.peek();
	}

	public void queueUpSong(Song s) {
		queue.add(s);
	}
}

public class CD{
	List<Song> list;
	String artist;
	public CD(List<Song> list, String artist) {
		this.list = list;
		this.artist = artist;
	}
}

public class Song {

}

public class User {
	private String name;
	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	private long ID;
	public long getID() {
		return ID;
	}

	public void setID(long ID) {
		this.ID = ID;
	}

	public User(long ID, String name) {
		this.name = name;
		this.ID = ID;
	}

	public User getUser() {
		return this;
	}

	public static User addUser(String name, long iD){}
}
```