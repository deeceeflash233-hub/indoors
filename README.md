import hashlib
import re

# Mock database to simulate storage
user_db = {}

def validate_email(email):
    """Simple regex to ensure email format is valid."""
    pattern = r'^[a-z0-9]+[\._]?[a-z0-9]+[@]\w+[.]\w+$'
    return re.match(pattern, email)

def hash_password(password):
    """Hashes a password using SHA-256 (Simplified for this example)."""
    return hashlib.sha256(password.encode()).hexdigest()

def register_user(email, password, country):
    """Handles user registration with basic U.S. compliance logic (validation)."""
    if not validate_email(email):
        print("Registration failed: Invalid email format.")
        return False
    
    if email in user_db:
        print("Registration failed: User already exists.")
        return False

    # In a real U.S. app, ensure you store a salted hash, never plain text
    user_db[email] = {
        "password": hash_password(password),
        "country": country,
        "verified": False  # Initial state for backend verification
    }
    print(f"Successfully registered: {email} from {country}")
    return True

def login_user(email, password):
    """Handles the login/verification process with error handling."""
    try:
        # 1. Check if user exists
        if email not in user_db:
            raise ValueError("Account not found.")

        # 2. Verify password match
        hashed_input = hash_password(password)
        if user_db[email]["password"] != hashed_input:
            raise Exception("Incorrect password.")

        return "Login successful"

    except ValueError as e:
        return f"Login failed: {e}"
    except Exception as e:
        # Catch-all for security or logic errors
        return f"Login failed: {str(e)}"

# --- Quick Test ---
register_user("dev_test@example.com", "SecurePass123", "USA")
print(login_user("dev_test@example.com", "SecurePass123"))
 # Backend verification logic in Python conceptual pseudo-code

def validate_registration(data):
    """Validates incoming registration data."""
    if not data.get('email') or not data.get('password') or not data.get('country'):
        return False, "Missing fields"
    # Add more complex email and password validation (e.g., regex, length)
    return True, "Validation successful"

def hash_password(password):
    """Securely hashes the password."""
    # Use a library like passlib or bcrypt for robust hashing
    import bcrypt
    return bcrypt.hashpw(password.encode('utf-8'), bcrypt.gensalt())

def verify_password(password, hashed_password):
    """Verifies the password against the stored hash."""
    import bcrypt
    return bcrypt.checkpw(password.encode('utf-8'), hashed_password)

def handle_registration(data):
    """Handles the full registration process."""
    valid, message = validate_registration(data)
    if not valid:
        return {"status": "error", "message": message}

    # Conceptual: check if user already exists in database
    # if user_exists(data['email']):
    #     return {"status": "error", "message": "User already exists"}

    hashed_pw = hash_password(data['password'])
    # Conceptual: store user in database with hashed_pw and country
    # user_id = save_user(data['email'], hashed_pw, data['country'])

    # Conceptual: send verification email (token generation and sending)
    # send_verification_email(data['email'], user_id)

    return {"status": "success", "message": "Registration successful, verification email sent"}

def handle_login(data):
    """Handles the login and session management."""
    # Conceptual: retrieve user from database based on email
    # user = get_user(data['email'])
    # if not user:
    #     return {"status": "error", "message": "Incorrect email or password"}

    # Conceptual: verify password
    # if not verify_password(data['password'], user['hashed_password']):
    #     return {"status": "error", "message": "Incorrect email or password"}

    # Conceptual: create and manage session
    # create_session(user['user_id'])

    return {"status": "success", "message": "Login successful"}
import hashlib

# Extended database simulation for NetIdentity
user_profiles = {}

def create_profile(email, password, country, bio=""):
    """Creates a detailed user profile with social foundations."""
    if email in user_profiles:
        print(f"Error: Profile already exists for {email}")
        return False

    # Securely hash the password
    hashed_password = hashlib.sha256(password.encode()).hexdigest()

    user_profiles[email] = {
        "credentials": hashed_password,
        "metadata": {
            "country": country,
            "bio": bio,
            "theme": "Mystery of Plants" # The aesthetic of the app
        },
        "social": {
            "friends": [],
            "pending_requests": []
        },
        "menu_access": ["Profile", "Plant Gallery", "Friend Finder", "Settings"],
        "verified": False
    }
    
    print(f"--- NetIdentity: Profile created for {email} ---")
    return True

def add_friend(user_email, friend_email):
    """Handles the friendship logic between two users."""
    if user_email not in user_profiles or friend_email not in user_profiles:
        print("One or both users do not exist.")
        return False
    
    # Check if they are already friends
    if friend_email in user_profiles[user_email]["social"]["friends"]:
        print(f"{friend_email} is already in your friends list.")
        return False

    # Mutual addition (Simplified for this model)
    user_profiles[user_email]["social"]["friends"].append(friend_email)
    user_profiles[friend_email]["social"]["friends"].append(user_email)
    print(f"Success: {user_email} and {friend_email} are now connected!")
    return True

def display_menu(email):
    """Displays the UI 'Face' for the user."""
    if email in user_profiles:
        user = user_profiles[email]
        print(f"\nWelcome to NetIdentity, {email}!")
        print(f"Menu: { ' | '.join(user['menu_access']) }")
        print(f"Friends ({len(user['social']['friends'])}): {user['social']['friends']}")
    else:
        print("User not logged in.")

# --- Running the Logic ---

# 1. Create Profiles
create_profile("lily@botany.com", "greenThumb1", "Ghana", bio="Lover of ferns.")
create_profile("rose@garden.com", "petalPower2", "USA", bio="Exploring rare fungi.")

# 2. Build the Social Network (Friends List)
add_friend("lily@botany.com", "rose@garden.com")

# 3. View the "Face" of the App (Menu & Friends)
display_menu("lily@botany.com")
import hashlib
import datetime

class NetIdentity:
    def __init__(self):
        self.user_database = {}
        self.system_logs = []  # To handle the verification process tracking

    def _generate_hash(self, password):
        """Private method to secure passwords."""
        return hashlib.sha256(password.encode()).hexdigest()

    def _log_event(self, message):
        """Internal log to track backend activity."""
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        self.system_logs.append(f"[{timestamp}] {message}")

    def register_user(self, email, password, country, bio=""):
        """Creates the profile and initializes the 'Face' of the app."""
        if email in self.user_database:
            print(f"!! Account {email} already exists.")
            return False

        self.user_database[email] = {
            "password": self._generate_hash(password),
            "profile": {
                "country": country,
                "bio": bio,
                "theme": "Mystery of Plants",
                "friends": []
            },
            "menu": ["1. Profile", "2. Plant Friends", "3. Discovery", "4. Settings"],
            "is_verified": False
        }
        self._log_event(f"New Registration: {email} from {country}")
        print(f"Success: {email} is now part of the NetIdentity ecosystem.")
        return True

    def verify_account(self, email):
        """Simulates the backend verification process."""
        if email in self.user_database:
            self.user_database[email]["is_verified"] = True
            self._log_event(f"Verification Success: {email} is now a verified botanist.")
            return True
        return False

    def add_friend(self, user_a, user_b):
        """Connects two users in the database."""
        if user_a in self.user_database and user_b in self.user_database:
            self.user_database[user_a]["profile"]["friends"].append(user_b)
            self.user_database[user_b]["profile"]["friends"].append(user_a)
            self._log_event(f"Social Connection: {user_a} <-> {user_b}")
            print(f"Connection Made: {user_a} and {user_b} are now friends!")
        else:
            print("Error: One of these users does not exist.")

    def show_app_face(self, email):
        """Displays the 'Face' (Menu and Friends List) as requested."""
        user = self.user_database.get(email)
        if not user:
            return "Access Denied."

        status = "✓ Verified" if user["is_verified"] else "✗ Unverified"
        print(f"\n{'='*30}")
        print(f"NETIDENTITY - {user['profile']['theme']}")
        print(f"User: {email} | {status}")
        print(f"{'='*30}")
        print("MAIN MENU:")
        for item in user["menu"]:
            print(f"  {item}")
        print("-" * 30)
        print(f"FRIENDS LIST ({len(user['profile']['friends'])}):")
        for friend in user['profile']['friends']:
            print(f"  • {friend}")
        print(f"{'='*30}\n")

# --- Execution ---
app = NetIdentity()

# 1. Registration
app.register_user("botanist_jane@nature.com", "pass123", "Ghana", "I love orchids.")
app.register_user("forest_guy@trees.com", "tree456", "USA", "Looking for ancient redwoods.")

# 2. Verification (Backend Log)
app.verify_account("botanist_jane@nature.com")

# 3. Social Interaction
app.add_friend("botanist_jane@nature.com", "forest_guy@trees.com")

# 4. Display the "Face" of the App
app.show_app_face("botanist_jane@nature.com")

# 5. Check the Backend Logs
print("DEBUG LOGS:", app.system_logs)

import hashlib
import datetime

class NetIdentityChat:
    def __init__(self, net_identity_instance):
        self.net_identity = net_identity_instance
        self.messages = []
        self._log = net_identity_instance.system_logs

    def _encrypt(self, message):
        """Conceptual E2E encryption simulation."""
        # In a real app, this would use sophisticated cryptographic libraries (e.g., NaCl, cryptography)
        # This is a placeholder to represent the commitment to E2E
        return f"E2E_ENCRYPTED:{hashlib.sha256(message.encode()).hexdigest()}"

    def _decrypt(self, encrypted_message):
        """Conceptual decryption."""
        # Decryption logic placeholder
        return f"DECRYPTED:{encrypted_message[-10:]}"

    def send_message(self, sender_email, recipient_email, message_content):
        """Sends a message, checking friend status and applying E2E encryption."""
        
        # 1. Validation: Check if both users exist
        if sender_email not in self.net_identity.user_database or recipient_email not in self.net_identity.user_database:
            print("Chat Error: Sender or recipient not found.")
            return False

        # 2. Authorization: Check if recipient is in sender's friends list
        friends = self.net_identity.user_database[sender_email]["profile"]["friends"]
        if recipient_email not in friends:
            print(f"Chat Error: {recipient_email} is not in {sender_email}'s friends list.")
            return False

        # 3. Process: Encrypt and store the message
        encrypted_content = self._encrypt(message_content)
        timestamp = datetime.datetime.now()

        self.messages.append({
            "timestamp": timestamp,
            "sender": sender_email,
            "recipient": recipient_email,
            "content": encrypted_content
        })

        self._log.append(f"[{timestamp.strftime('%Y-%m-%d %H:%M:%S')}] CHAT: Encrypted message sent from {sender_email} to {recipient_email}.")
        print(f"Message sent securely from {sender_email} to {recipient_email}.")
        return True

    def view_recent_messages(self, email):
        """Views recent encrypted messages for a user, conceptually decrypting them."""
        print(f"\n--- Encrypted Chat History for {email} ---")
        found = False
        for msg in self.messages:
            if msg['recipient'] == email or msg['sender'] == email:
                # Conceptual decryption for display
                decrypted_content = self._decrypt(msg['content'])
                print(f"[{msg['timestamp'].strftime('%H:%M')}] From {msg['sender']} to {msg['recipient']}: {decrypted_content}")
                found = True
        if not found:
            print("No recent messages.")

# --- Integrated Execution (Requires the previous NetIdentity class setup) ---
# Assuming 'app' instance from previous code exists:
# app = NetIdentity()
# app.register_user("botanist_jane@nature.com", "pass123", "Ghana", "I love orchids.")
# app.register_user("forest_guy@trees.com", "tree456", "USA", "Looking for ancient redwoods.")
# app.add_friend("botanist_jane@nature.com", "forest_guy@trees.com")

# chat_system = NetIdentityChat(app)

# # Send a secure, E2E conceptual message
# chat_system.send_message("botanist_jane@nature.com", "forest_guy@trees.com", "I found a rare species of fern!")

# # View messages
# chat_system.view_recent_messages("forest_guy@trees.com")

import datetime

class Monetization:
    def __init__(self, net_identity_instance):
        self.net_identity = net_identity_instance
        self.subscription_plans = {
            "Basic": {"price": 0, "features": ["Standard Profile", "Friends List", "Basic Search"]},
            "Premium": {"price": 9.99, "features": ["Standard Profile", "Friends List", "Advanced Search", "Exclusive Plant Gallery", "Encrypted Group Chat"]}
        }
        # Tracks active subscriptions: {email: {"plan": "PlanName", "expiry": datetime}}
        self.active_subscriptions = {}
        self._log = net_identity_instance.system_logs

    def subscribe(self, email, plan_name):
        """Handles subscription purchase and validation."""
        if email not in self.net_identity.user_database:
            print(f"Monetization Error: User {email} not found.")
            return False

        if plan_name not in self.subscription_plans:
            print(f"Monetization Error: Plan {plan_name} not available.")
            return False

        # Conceptual payment processing placeholder
        print(f"Processing payment for {plan_name} plan for user {email}...")
        
        # Set expiry date (e.g., 1 month from now)
        expiry_date = datetime.datetime.now() + datetime.timedelta(days=30)
        
        self.active_subscriptions[email] = {
            "plan": plan_name,
            "expiry": expiry_date
        }
        
        self._log.append(f"[{datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')}] MONETIZATION: {email} subscribed to {plan_name} until {expiry_date}.")
        print(f"Successfully subscribed {email} to {plan_name} plan.")
        return True

    def check_subscription_status(self, email):
        """Checks if a user has an active, valid subscription."""
        subscription = self.active_subscriptions.get(email)
        if not subscription:
            return None, False

        if datetime.datetime.now() > subscription["expiry"]:
            self._log.append(f"[{datetime.datetime.now().strftime('%Y-%m-%m %H:%M:%S')}] MONETIZATION: {email} subscription expired.")
            return subscription["plan"], False
        
        return subscription["plan"], True

    def has_premium_access(self, email):
        """Determines if a user can access premium features."""
        plan, is_active = self.check_subscription_status(email)
        return is_active and plan == "Premium"

# --- Integrated Execution (Requires previous NetIdentity class) ---
# Assuming 'app' instance from previous code exists and users are registered
# app = NetIdentity()
# app.register_user("botanist_jane@nature.com", "pass123", "Ghana", "I love orchids.")
# app.register_user("forest_guy@trees.com", "tree456", "USA", "Looking for ancient redwoods.")

# monetization_system = Monetization(app)

# # Subscribe a user to the Premium plan
# monetization_system.subscribe("botanist_jane@nature.com", "Premium")

# # Check premium access before allowing specific features (e.g., advanced search)
# if monetization_system.has_premium_access("botanist_jane@nature.com"):
#     print("Botanist Jane has Premium access to Exclusive Plant Gallery!")
# else:
#     print("Botanist Jane does not have Premium access.")

# # Check logs for monetization events
# print("MONETIZATION LOGS:", app.system_logs)

import hashlib
import datetime

class NetIdentity:
    """Core NetIdentity platform managing profiles, social, monetization, and extended features."""
    def __init__(self):
        self.user_database = {}
        self.system_logs = []
        self.subscription_plans = {
            "Basic": {"price": 0, "features": ["Standard Profile", "Friends List"]},
            "Premium": {"price": 9.99, "features": ["Advanced Search", "Exclusive Plant Gallery", "Encrypted Group Chat"]}
        }
        self.active_subscriptions = {}
        self.plant_database = {} # New: Central database for user discoveries
        self.community_forum = [] # New: Central forum for posts

    def _generate_hash(self, password):
        return hashlib.sha256(password.encode()).hexdigest()

    def _log_event(self, message):
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        self.system_logs.append(f"[{timestamp}] {message}")

    def register_user(self, email, password, country, bio=""):
        if email in self.user_database:
            print(f"!! Account {email} already exists.")
            return False
        
        self.user_database[email] = {
            "password": self._generate_hash(password),
            "profile": {
                "country": country,
                "bio": bio,
                "theme": "Mystery of Plants",
                "friends": []
            },
            "menu": ["1. Profile", "2. Plant Friends", "3. Discovery", "4. Settings"],
            "is_verified": False,
            "user_plants": [] # Link user to their discoveries
        }
        self._log_event(f"New Registration: {email} from {country}")
        print(f"Success: {email} is now part of the NetIdentity ecosystem.")
        return True

    # ... (Monetization and Chat methods from previous steps can be integrated here) ...

    def add_plant_discovery(self, email, plant_name, location):
        """Allows verified users to add to the conceptual Plant Database."""
        if email not in self.user_database or not self.user_database[email]["is_verified"]:
            print(f"Error: {email} must be verified to add plant discoveries.")
            return False

        discovery = {
            "plant_name": plant_name,
            "location": location,
            "discovered_by": email,
            "timestamp": datetime.datetime.now()
        }
        self.plant_database[plant_name] = discovery
        self.user_database[email]["user_plants"].append(plant_name)
        self._log_event(f"Discovery: {email} added new plant {plant_name}")
        print(f"Discovery added: {plant_name} recorded.")
        return True

    def post_to_forum(self, email, subject, content):
        """Enables community interaction in the forum."""
        if email not in self.user_database:
            print("Error: User not found.")
            return False

        post = {
            "author": email,
            "subject": subject,
            "content": content,
            "timestamp": datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        }
        self.community_forum.append(post)
        self._log_event(f"Forum: {email} created new post '{subject}'")
        print(f"Post created successfully in Community Forum.")

    def show_app_face(self, email):
        """Displays the updated 'Face' including new features."""
        user = self.user_database.get(email)
        if not user:
            return "Access Denied."
        
        status = "✓ Verified" if user["is_verified"] else "✗ Unverified"
        print(f"\n--- NETIDENTITY: Mystery of Plants ---")
        print(f"Welcome, {email} | Status: {status}")
        print(f"Menu: { ' | '.join(user['menu']) }")
        print(f"Discoveries: {len(user['user_plants'])} plants recorded.")
        print(f"Forum Posts: {len(self.community_forum)} total.")
        print("-" * 35)


# --- Execution Example ---
app = NetIdentity()
app.register_user("botanist_jane@nature.com", "pass123", "Ghana", "I love orchids.")
app.verify_account("botanist_jane@nature.com") # Must verify to use discovery

# New Features in action
app.add_plant_discovery("botanist_jane@nature.com", "Ghost Orchid", "A secret forest in Ghana")
app.post_to_forum("botanist_jane@nature.com", "Rare Orchid Spotting", "Found a stunning Ghost Orchid today!")

app.show_app_face("botanist_jane@nature.com")
print("SYSTEM LOGS (excerpt):", app.system_logs[-2:])
