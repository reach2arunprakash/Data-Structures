# OnlineReaderSystem

```java
public class OnlineReaderSystem {
	private Library library;
	private UserManager userManager;
	private Display display;
	private Book activeBook;
	private User activeUser;

	public OnlineReaderSystem() {
		userManager = new UserManager();
		library = new Library();
		display = new Display();
	}

	public Library getLibrary() {
		return library;
	}

	public UserManager getUserManager() {
		return userManager;
	}

	public Display getDisplay() {
		return display;
	}

	public Book getActiveBook() {
		return activeBook;
	}

	public void setActiveBook(Book book) {
		activeBook = book;
		display.displayBook(book);
	}

	public User getActiveUser() {
		return activeUser;
	}

	public void setUser(User user) {
		activeUser = user;
	}

}

public class Library {
	Private HashTable<Integer, Book> books;

	public Book addBook() {

	}

	public boolean remove(Book b){}
	public boolean remove(int id){}

	public Book find(int id) {}
}

public class UserManager {
	private Hashtable<Integer, User> users;

	public User addUser(int id, String details, int accountType) {
		if (users.containsKey(id)) {
			return null;
		}

		User user = new User(id, details, accountType);
		user.put(id, user);
		return user;
	}

	public boolean remove(User u) {
		return remove(u.getID());
	}

	public boolean remove(int id) {
		if (!users.containsKey(id)) {
			return false;
		}

		return users.remove(id);
	}

	public User find(int id) {
		return users.get(id);
	}
}

public class Display {
	private Book activeBook;
	private User activeUser;
	private int pageNumber = 0;

	public void displayUser(User user){}
	public void displayBook(Book book){}
	public void turnPageForward() {}
	public void turnPageBackward(){}

	// Refresh functions
}

public class Book {
	private int bookId;
	private String details;

	public Book (int id, int det) {}

	public int getID(){}
	public void setID(){}
	public String getDetails(){}
	public void setDetails(){}
}

public class User {
	private int userId;
	private String details;
	private int accountType;

	public void renewMembership() {}

	public User (int id, String details, int accountType){}

	//getters and setters
}
```