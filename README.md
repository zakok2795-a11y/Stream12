<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>StreamFlix - Watch Movies Online</title>
<style>
/* Reset & Body */
body, html { margin:0; padding:0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background-color:#121212; color:#fff; }
a { text-decoration:none; color:#fff; }
.container { max-width:1200px; margin:auto; padding:20px; }

/* Navbar */
.navbar { display:flex; justify-content:space-between; align-items:center; padding:15px 20px; background:#1a1a1a; position:sticky; top:0; z-index:1000; box-shadow: 0 2px 10px rgba(0,0,0,0.5); }
.navbar .logo { font-size:28px; font-weight:bold; color: #f39c12; }
.navbar .menu { display: flex; align-items: center; }
.navbar .menu a { margin-left:25px; transition:0.2s; font-weight:500; }
.navbar .menu a:hover { color:#f39c12; }
.navbar .menu a.btn-login { background: #f39c12; color: #000; padding: 8px 16px; border-radius: 4px; font-weight: bold; }
.navbar .menu a.btn-login:hover { background: #e67e22; color: #000; }

/* Search Bar */
.search-container { margin:20px 0; text-align:center; position: relative; }
.search-container input { width:80%; max-width: 500px; padding:12px 20px; border-radius:30px; border:none; font-size:16px; background: #2c2c2c; color: white; padding-left: 45px; }
.search-container i { position: absolute; left: 15%; top: 50%; transform: translateY(-50%); color: #888; }
.search-results { display: none; position: absolute; background: #2c2c2c; width: 80%; max-width: 500px; left: 50%; transform: translateX(-50%); border-radius: 0 0 8px 8px; z-index: 100; max-height: 300px; overflow-y: auto; box-shadow: 0 4px 12px rgba(0,0,0,0.5); }
.search-results.active { display: block; }
.search-item { padding: 12px 20px; display: flex; align-items: center; border-bottom: 1px solid #3c3c3c; cursor: pointer; }
.search-item:hover { background: #3c3c3c; }
.search-item img { width: 40px; height: 60px; object-fit: cover; border-radius: 4px; margin-right: 15px; }
.search-item-info h4 { margin: 0 0 5px 0; font-size: 16px; }
.search-item-info p { margin: 0; color: #aaa; font-size: 14px; }

/* Hero Section */
.hero { position: relative; height: 500px; margin: 0 -20px 30px -20px; overflow: hidden; border-radius: 0 0 10px 10px; }
.hero-image { width: 100%; height: 100%; object-fit: cover; filter: brightness(0.7); }
.hero-content { position: absolute; bottom: 0; left: 0; padding: 30px; max-width: 600px; }
.hero-title { font-size: 2.5rem; margin-bottom: 10px; font-weight: bold; }
.hero-description { font-size: 1rem; margin-bottom: 20px; color: #ddd; line-height: 1.5; }
.hero-actions { display: flex; gap: 15px; }
.hero-btn { padding: 12px 25px; border-radius: 4px; font-weight: bold; cursor: pointer; border: none; font-size: 1rem; display: flex; align-items: center; gap: 8px; }
.hero-btn.play { background: #f39c12; color: #000; }
.hero-btn.info { background: rgba(100, 100, 100, 0.7); color: white; }
.hero-btn:hover { opacity: 0.9; }

/* Carousel / Featured */
.section-title { font-size: 1.5rem; margin: 30px 0 15px 0; padding-bottom: 10px; border-bottom: 2px solid #f39c12; display: inline-block; }
.carousel-container { position: relative; margin-bottom: 40px; }
.carousel { display: flex; overflow-x: auto; gap: 15px; padding: 10px 5px 20px 5px; scroll-behavior: smooth; -ms-overflow-style: none; scrollbar-width: none; }
.carousel::-webkit-scrollbar { display: none; }
.carousel-item { flex: 0 0 auto; width: 250px; border-radius: 10px; overflow: hidden; position: relative; transition: transform 0.3s; }
.carousel-item img { width: 100%; height: 350px; object-fit: cover; display: block; }
.carousel-item:hover { transform: scale(1.05); z-index: 2; }
.carousel-item-overlay { position: absolute; bottom: 0; left: 0; right: 0; background: linear-gradient(transparent, rgba(0,0,0,0.8)); padding: 15px; color: white; }
.carousel-item-title { font-weight: bold; margin-bottom: 5px; }
.carousel-item-info { display: flex; justify-content: space-between; font-size: 0.85rem; color: #ddd; }
.carousel-nav { position: absolute; top: 50%; transform: translateY(-50%); width: 40px; height: 40px; background: rgba(0,0,0,0.5); border-radius: 50%; display: flex; align-items: center; justify-content: center; cursor: pointer; z-index: 3; border: none; color: white; font-size: 1.2rem; }
.carousel-nav.prev { left: 10px; }
.carousel-nav.next { right: 10px; }
.carousel-nav:hover { background: rgba(0,0,0,0.8); }

/* Categories */
.categories { display:flex; gap:15px; flex-wrap:wrap; margin-bottom:30px; justify-content: center; }
.categories a { flex: 1 1 150px; background:#2c2c2c; padding:15px; text-align:center; border-radius:8px; transition:0.2s; font-weight: 500; display: flex; align-items: center; justify-content: center; gap: 8px; }
.categories a:hover { background:#f39c12; color:#000; transform: translateY(-3px); }

/* Movie Grid */
.movie-grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap:20px; margin-bottom:40px; }
.movie-card { border-radius:10px; overflow:hidden; position: relative; transition: transform 0.3s; background: #1c1c1c; }
.movie-card img { width:100%; height:300px; object-fit: cover; display:block; }
.movie-card:hover { transform: scale(1.05); z-index: 2; box-shadow: 0 5px 15px rgba(0,0,0,0.5); }
.movie-info { padding: 15px; }
.movie-title { font-weight: bold; margin-bottom: 8px; font-size: 1rem; }
.movie-meta { display: flex; justify-content: space-between; color: #aaa; font-size: 0.85rem; }

/* Movie Detail */
.movie-detail { display:flex; flex-wrap:wrap; gap:30px; margin-bottom:40px; }
.movie-detail .poster { flex:1 1 300px; }
.movie-detail .poster img { width:100%; border-radius:10px; box-shadow: 0 5px 15px rgba(0,0,0,0.5); }
.movie-detail .info { flex:2 1 600px; }
.movie-detail .info h1 { margin-top:0; font-size: 2.2rem; color: #f39c12; }
.movie-meta-detail { display: flex; gap: 15px; margin: 15px 0; flex-wrap: wrap; }
.meta-item { background: #2c2c2c; padding: 5px 12px; border-radius: 20px; font-size: 0.9rem; }
.movie-description { line-height: 1.6; margin-bottom: 20px; color: #ddd; }
.movie-actions { display: flex; gap: 15px; margin: 20px 0; }
.action-btn { padding: 12px 25px; border-radius: 4px; font-weight: bold; cursor: pointer; border: none; font-size: 1rem; display: flex; align-items: center; gap: 8px; }
.action-btn.primary { background: #f39c12; color: #000; }
.action-btn.secondary { background: transparent; border: 1px solid #f39c12; color: #f39c12; }
.action-btn:hover { opacity: 0.9; }
.player-container { width:100%; height:500px; border:none; border-radius:10px; margin-top:20px; background: #000; position: relative; }
.player-placeholder { width: 100%; height: 100%; display: flex; align-items: center; justify-content: center; flex-direction: column; background: #0a0a0a; border-radius: 10px; }
.player-placeholder i { font-size: 64px; color: #f39c12; margin-bottom: 20px; }
.player-placeholder p { color: #aaa; margin-bottom: 20px; }
.player-placeholder .action-btn { margin-top: 10px; }

/* Recommended */
.recommended { margin-top:40px; }
.recommended h2 { margin-bottom:20px; font-size: 1.5rem; padding-bottom: 10px; border-bottom: 2px solid #f39c12; }
.recommended-movies { display:flex; gap:15px; overflow-x:auto; padding: 10px 5px; scroll-behavior: smooth; }
.recommended-movies::-webkit-scrollbar { height: 6px; }
.recommended-movies::-webkit-scrollbar-track { background: #2c2c2c; border-radius: 10px; }
.recommended-movies::-webkit-scrollbar-thumb { background: #f39c12; border-radius: 10px; }
.recommended-movie { flex: 0 0 auto; width: 150px; border-radius: 8px; overflow: hidden; transition: transform 0.2s; }
.recommended-movie img { width: 100%; height: 225px; object-fit: cover; }
.recommended-movie:hover { transform: scale(1.05); }

/* Footer */
footer { background:#1a1a1a; text-align:center; padding:30px; margin-top:60px; font-size:14px; }
.footer-content { max-width: 800px; margin: 0 auto; }
.footer-links { display: flex; justify-content: center; gap: 20px; margin: 15px 0; flex-wrap: wrap; }
.footer-links a { color: #aaa; transition: color 0.2s; }
.footer-links a:hover { color: #f39c12; }
.social-icons { margin: 20px 0; display: flex; justify-content: center; gap: 15px; }
.social-icons a { display: inline-flex; align-items: center; justify-content: center; width: 40px; height: 40px; border-radius: 50%; background: #2c2c2c; transition: background 0.2s; }
.social-icons a:hover { background: #f39c12; }
.copyright { color: #777; margin-top: 20px; }

/* Modal */
.modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 10000; align-items: center; justify-content: center; }
.modal-content { background: #1c1c1c; border-radius: 10px; width: 90%; max-width: 800px; max-height: 90vh; overflow: hidden; position: relative; }
.modal-close { position: absolute; top: 15px; right: 15px; background: rgba(0,0,0,0.5); width: 30px; height: 30px; border-radius: 50%; display: flex; align-items: center; justify-content: center; cursor: pointer; z-index: 2; }
.modal-body { padding: 20px; overflow-y: auto; max-height: calc(90vh - 40px); }

/* Responsive */
@media (max-width: 768px) {
    .navbar .menu a:not(.btn-login) { display: none; }
    .navbar .menu { position: relative; }
    .navbar .menu .mobile-toggle { display: block; font-size: 1.5rem; cursor: pointer; }
    .hero { height: 400px; }
    .hero-title { font-size: 2rem; }
    .movie-grid { grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); }
    .carousel-item { width: 200px; }
    .carousel-item img { height: 300px; }
    .movie-detail { flex-direction: column; }
    .search-container input { width: 90%; padding-left: 40px; }
    .search-container i { left: 5%; }
}

@media (max-width: 480px) {
    .hero { height: 350px; }
    .hero-title { font-size: 1.8rem; }
    .hero-btn { padding: 10px 15px; font-size: 0.9rem; }
    .categories a { flex: 1 1 120px; padding: 12px; font-size: 0.9rem; }
    .movie-grid { grid-template-columns: repeat(2, 1fr); gap: 15px; }
    .carousel-item { width: 180px; }
    .carousel-item img { height: 250px; }
    .recommended-movie { width: 120px; }
    .recommended-movie img { height: 180px; }
}

/* Loading animation */
.loading { display: flex; justify-content: center; align-items: center; height: 200px; }
.spinner { width: 40px; height: 40px; border: 4px solid rgba(255,255,255,0.1); border-radius: 50%; border-top: 4px solid #f39c12; animation: spin 1s linear infinite; }
@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

/* Toast notification */
.toast { position: fixed; bottom: 20px; right: 20px; background: #f39c12; color: #000; padding: 12px 20px; border-radius: 4px; box-shadow: 0 2px 10px rgba(0,0,0,0.2); z-index: 1000; opacity: 0; transform: translateY(100px); transition: all 0.3s; }
.toast.show { opacity: 1; transform: translateY(0); }
</style>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>

<!-- Navbar -->
<div class="navbar">
    <div class="logo">StreamFlix</div>
    <div class="menu">
        <a href="#">Home</a>
        <a href="#">Categories</a>
        <a href="#">Trending</a>
        <a href="#" class="btn-login">Login</a>
        <a href="javascript:void(0)" class="mobile-toggle"><i class="fas fa-bars"></i></a>
    </div>
</div>

<div class="container">
    <!-- Search -->
    <div class="search-container">
        <i class="fas fa-search"></i>
        <input type="text" id="search-input" placeholder="Search movies...">
        <div class="search-results" id="search-results"></div>
    </div>

    <!-- Hero Section -->
    <div class="hero">
        <img src="https://source.unsplash.com/random/1200x500/?movie,cinema" alt="Featured Movie" class="hero-image">
        <div class="hero-content">
            <h2 class="hero-title">The Midnight Adventure</h2>
            <p class="hero-description">In a world where dreams become reality, a group of friends discover they have the power to change their fate. An epic journey of courage and friendship.</p>
            <div class="hero-actions">
                <button class="hero-btn play"><i class="fas fa-play"></i> Play</button>
                <button class="hero-btn info"><i class="fas fa-info-circle"></i> More Info</button>
            </div>
        </div>
    </div>

    <!-- Featured Carousel -->
    <h2 class="section-title">Featured Movies</h2>
    <div class="carousel-container">
        <button class="carousel-nav prev"><i class="fas fa-chevron-left"></i></button>
        <button class="carousel-nav next"><i class="fas fa-chevron-right"></i></button>
        <div class="carousel" id="featured-carousel">
            <!-- Content will be loaded by JavaScript -->
        </div>
    </div>

    <!-- Categories -->
    <div class="categories">
        <a href="#"><i class="fas fa-fire"></i> Action</a>
        <a href="#"><i class="fas fa-theater-masks"></i> Drama</a>
        <a href="#"><i class="fas fa-laugh"></i> Comedy</a>
        <a href="#"><i class="fas fa-dragon"></i> Animation</a>
        <a href="#"><i class="fas fa-ghost"></i> Horror</a>
        <a href="#"><i class="fas fa-rocket"></i> Sci-Fi</a>
    </div>

    <!-- Movie Grid -->
    <h2 class="section-title">Popular Now</h2>
    <div class="movie-grid" id="movie-grid">
        <!-- Content will be loaded by JavaScript -->
    </div>

    <!-- Recommended Movies -->
    <div class="recommended">
        <h2>Recommended For You</h2>
        <div class="recommended-movies" id="recommended-movies">
            <!-- Content will be loaded by JavaScript -->
        </div>
    </div>
</div>

<!-- Footer -->
<footer>
    <div class="footer-content">
        <div class="social-icons">
            <a href="#"><i class="fab fa-facebook-f"></i></a>
            <a href="#"><i class="fab fa-twitter"></i></a>
            <a href="#"><i class="fab fa-instagram"></i></a>
            <a href="#"><i class="fab fa-youtube"></i></a>
        </div>
        <div class="footer-links">
            <a href="#">Home</a>
            <a href="#">Movies</a>
            <a href="#">TV Shows</a>
            <a href="#">My List</a>
            <a href="#">About Us</a>
            <a href="#">Contact</a>
            <a href="#">Privacy Policy</a>
            <a href="#">Terms of Service</a>
        </div>
        <div class="copyright">
            &copy; 2025 StreamFlix. All Rights Reserved.
        </div>
    </div>
</footer>

<!-- Movie Detail Modal -->
<div class="modal" id="movie-modal">
    <div class="modal-content">
        <div class="modal-close" id="modal-close"><i class="fas fa-times"></i></div>
        <div class="modal-body" id="modal-body">
            <!-- Content will be loaded by JavaScript -->
        </div>
    </div>
</div>

<!-- Toast Notification -->
<div class="toast" id="toast"></div>

<script>
// Sample movie data
const movies = [
    {
        id: 1,
        title: "The Midnight Adventure",
        year: 2025,
        genre: ["Action", "Adventure", "Sci-Fi"],
        duration: "120 min",
        rating: 4.5,
        description: "In a world where dreams become reality, a group of friends discover they have the power to change their fate. An epic journey of courage and friendship.",
        poster: "https://source.unsplash.com/random/300x450/?movie",
        thumbnail: "https://source.unsplash.com/random/300x450/?movie",
        featured: true
    },
    {
        id: 2,
        title: "Dark Shadows",
        year: 2024,
        genre: ["Horror", "Thriller"],
        duration: "105 min",
        rating: 4.2,
        description: "A family moves into a seemingly perfect house, only to discover it holds dark secrets that threaten to consume them.",
        poster: "https://source.unsplash.com/random/300x450/?horror",
        thumbnail: "https://source.unsplash.com/random/300x450/?horror",
        featured: true
    },
    {
        id: 3,
        title: "Laugh Out Loud",
        year: 2023,
        genre: ["Comedy"],
        duration: "95 min",
        rating: 3.9,
        description: "A stand-up comedian's life turns upside down when he accidentally becomes the guardian of his eccentric aunt's prized poodle.",
        poster: "https://source.unsplash.com/random/300x450/?comedy",
        thumbnail: "https://source.unsplash.com/random/300x450/?comedy",
        featured: true
    },
    {
        id: 4,
        title: "Space Odyssey",
        year: 2025,
        genre: ["Sci-Fi", "Adventure"],
        duration: "135 min",
        rating: 4.7,
        description: "Astronauts on a mission to colonize a distant planet encounter mysterious phenomena that challenge their understanding of reality.",
        poster: "https://source.unsplash.com/random/300x450/?space",
        thumbnail: "https://source.unsplash.com/random/300x450/?space"
    },
    {
        id: 5,
        title: "The Last Stand",
        year: 2024,
        genre: ["Action", "Drama"],
        duration: "118 min",
        rating: 4.3,
        description: "A retired special forces operative is forced back into action when his city is taken hostage by a ruthless criminal organization.",
        poster: "https://source.unsplash.com/random/300x450/?action",
        thumbnail: "https://source.unsplash.com/random/300x450/?action"
    },
    {
        id: 6,
        title: "Heartstrings",
        year: 2023,
        genre: ["Drama", "Romance"],
        duration: "127 min",
        rating: 4.0,
        description: "A talented but struggling musician finds inspiration and love in the most unexpected place during a life-changing summer.",
        poster: "https://source.unsplash.com/random/300x450/?drama",
        thumbnail: "https://source.unsplash.com/random/300x450/?drama"
    },
    {
        id: 7,
        title: "Mystery Mansion",
        year: 2024,
        genre: ["Mystery", "Thriller"],
        duration: "111 min",
        rating: 4.1,
        description: "Guests at an exclusive retreat find themselves trapped in a deadly game where they must solve puzzles to survive.",
        poster: "https://source.unsplash.com/random/300x450/?mystery",
        thumbnail: "https://source.unsplash.com/random/300x450/?mystery"
    },
    {
        id: 8,
        title: "Animation Adventure",
        year: 2025,
        genre: ["Animation", "Family"],
        duration: "99 min",
        rating: 4.6,
        description: A young inventor and her robotic companion embark on a fantastic journey to save their whimsical world from fading away.",
        poster: "https://source.unsplash.com/random/300x450/?animation",
        thumbnail: "https://source.unsplash.com/random/300x450/?animation"
    }
];

// DOM elements
const searchInput = document.getElementById('search-input');
const searchResults = document.getElementById('search-results');
const featuredCarousel = document.getElementById('featured-carousel');
const movieGrid = document.getElementById('movie-grid');
const recommendedMovies = document.getElementById('recommended-movies');
const movieModal = document.getElementById('movie-modal');
const modalBody = document.getElementById('modal-body');
const modalClose = document.getElementById('modal-close');
const toast = document.getElementById('toast');

// Initialize the page
document.addEventListener('DOMContentLoaded', function() {
    loadFeaturedMovies();
    loadMovieGrid();
    loadRecommendedMovies();
    
    // Set up event listeners
    searchInput.addEventListener('input', handleSearch);
    modalClose.addEventListener('click', closeModal);
    
    // Close modal when clicking outside
    window.addEventListener('click', function(event) {
        if (event.target === movieModal) {
            closeModal();
        }
    });
    
    // Carousel navigation
    document.querySelector('.carousel-nav.prev').addEventListener('click', function() {
        featuredCarousel.scrollBy({ left: -300, behavior: 'smooth' });
    });
    
    document.querySelector('.carousel-nav.next').addEventListener('click', function() {
        featuredCarousel.scrollBy({ left: 300, behavior: 'smooth' });
    });
});

// Load featured movies into carousel
function loadFeaturedMovies() {
    const featured = movies.filter(movie => movie.featured);
    
    featured.forEach(movie => {
        const carouselItem = document.createElement('div');
        carouselItem.className = 'carousel-item';
        carouselItem.innerHTML = `
            <img src="${movie.thumbnail}" alt="${movie.title}">
            <div class="carousel-item-overlay">
                <div class="carousel-item-title">${movie.title}</div>
                <div class="carousel-item-info">
                    <span>${movie.year}</span>
                    <span>${getStarRating(movie.rating)}</span>
                </div>
            </div>
        `;
        carouselItem.addEventListener('click', () => openMovieDetail(movie));
        featuredCarousel.appendChild(carouselItem);
    });
}

// Load movies into grid
function loadMovieGrid() {
    movies.forEach(movie => {
        const movieCard = document.createElement('div');
        movieCard.className = 'movie-card';
        movieCard.innerHTML = `
            <img src="${movie.thumbnail}" alt="${movie.title}">
            <div class="movie-info">
                <div class="movie-title">${movie.title}</div>
                <div class="movie-meta">
                    <span>${movie.year}</span>
                    <span>${getStarRating(movie.rating)}</span>
                </div>
            </div>
        `;
        movieCard.addEventListener('click', () => openMovieDetail(movie));
        movieGrid.appendChild(movieCard);
    });
}

// Load recommended movies
function loadRecommendedMovies() {
    // Shuffle movies for variety
    const shuffled = [...movies].sort(() => 0.5 - Math.random());
    
    // Take 6 random movies
    shuffled.slice(0, 6).forEach(movie => {
        const recMovie = document.createElement('div');
        recMovie.className = 'recommended-movie';
        recMovie.innerHTML = `<img src="${movie.thumbnail}" alt="${movie.title}">`;
        recMovie.addEventListener('click', () => openMovieDetail(movie));
        recommendedMovies.appendChild(recMovie);
    });
}

// Handle search functionality
function handleSearch() {
    const query = searchInput.value.toLowerCase().trim();
    
    if (query.length < 2) {
        searchResults.classList.remove('active');
        return;
    }
    
    const results = movies.filter(movie => 
        movie.title.toLowerCase().includes(query) ||
        movie.genre.some(g => g.toLowerCase().includes(query))
    );
    
    displaySearchResults(results);
}

// Display search results
function displaySearchResults(results) {
    searchResults.innerHTML = '';
    
    if (results.length === 0) {
        searchResults.innerHTML = '<div class="search-item">No results found</div>';
    } else {
        results.forEach(movie => {
            const resultItem = document.createElement('div');
            resultItem.className = 'search-item';
            resultItem.innerHTML = `
                <img src="${movie.thumbnail}" alt="${movie.title}">
                <div class="search-item-info">
                    <h4>${movie.title}</h4>
                    <p>${movie.year} • ${movie.genre.join(', ')}</p>
                </div>
            `;
            resultItem.addEventListener('click', () => {
                openMovieDetail(movie);
                searchResults.classList.remove('active');
                searchInput.value = '';
            });
            searchResults.appendChild(resultItem);
        });
    }
    
    searchResults.classList.add('active');
}

// Open movie detail modal
function openMovieDetail(movie) {
    modalBody.innerHTML = `
        <div class="movie-detail">
            <div class="poster">
                <img src="${movie.poster}" alt="${movie.title}">
            </div>
            <div class="info">
                <h1>${movie.title} (${movie.year})</h1>
                <div class="movie-meta-detail">
                    <span class="meta-item">${movie.genre.join(', ')}</span>
                    <span class="meta-item">${movie.duration}</span>
                    <span class="meta-item">${getStarRating(movie.rating)}</span>
                </div>
                <p class="movie-description">${movie.description}</p>
                <div class="movie-actions">
                    <button class="action-btn primary" onclick="playMovie(${movie.id})"><i class="fas fa-play"></i> Play</button>
                    <button class="action-btn secondary" onclick="addToWatchlist(${movie.id})"><i class="fas fa-plus"></i> Watchlist</button>
                </div>
                <div class="player-container">
                    <div class="player-placeholder">
                        <i class="fas fa-play-circle"></i>
                        <p>Click play to start watching</p>
                    </div>
                </div>
            </div>
        </div>
    `;
    movieModal.style.display = 'flex';
    document.body.style.overflow = 'hidden';
}

// Close modal
function closeModal() {
    movieModal.style.display = 'none';
    document.body.style.overflow = 'auto';
}

// Play movie (simulated)
function playMovie(movieId) {
    showToast("Playing movie...");
    // In a real app, this would trigger the actual video player
    const playerContainer = document.querySelector('.player-placeholder');
    if (playerContainer) {
        playerContainer.innerHTML = `
            <div class="loading">
                <div class="spinner"></div>
            </div>
            <p>Loading movie...</p>
        `;
        
        // Simulate loading
        setTimeout(() => {
            playerContainer.innerHTML = `
                <div style="width:100%;height:100%;display:flex;align-items:center;justify-content:center;background:#000;">
                    <div style="text-align:center;">
                        <i class="fas fa-play-circle" style="font-size:64px;color:#f39c12;"></i>
                        <p style="margin-top:20px;">Video would play here</p>
                        <button class="action-btn primary" style="margin-top:20px;" onclick="showToast('Movie playing in full player')">
                            <i class="fas fa-external-link-alt"></i> Open Full Player
                        </button>
                    </div>
                </div>
            `;
        }, 1500);
    }
}

// Add to watchlist
function addToWatchlist(movieId) {
    showToast("Added to your watchlist");
    // In a real app, this would save to user's watchlist
}

// Show toast notification
function showToast(message) {
    toast.textContent = message;
    toast.classList.add('show');
    
    setTimeout(() => {
        toast.classList.remove('show');
    }, 3000);
}

// Helper function to get star rating
function getStarRating(rating) {
    const fullStars = Math.floor(rating);
    const halfStar = rating % 1 >= 0.5;
    const emptyStars = 5 - fullStars - (halfStar ? 1 : 0);
    
    let stars = '';
    for (let i = 0; i < fullStars; i++) stars += '★';
    if (halfStar) stars += '⯨';
    for (let i = 0; i < emptyStars; i++) stars += '☆';
    
    return stars;
}
</script>
</body>
</html>
