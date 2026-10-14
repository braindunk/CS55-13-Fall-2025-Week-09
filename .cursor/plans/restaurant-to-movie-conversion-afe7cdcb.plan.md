<!-- afe7cdcb-1dfa-4584-bd3a-f9a26456a568 9e96b172-e783-4983-9b6e-e9dcd9bb69be -->
# Convert Restaurant Review Site to Movie Review Site

## Overview

Transform the Friendly Eats restaurant review application into a movie review platform with appropriate movie-specific terminology, data structures, and filters.

## Data Model Changes

### Core Entity: Restaurant → Movie

Update Firestore collection and data structure:

- Collection: `restaurants` → `movies`
- Fields mapping:
- `name` → `title` (movie title)
- `category` → `genre` (movie genre)
- `city` → `streamingService` (Netflix, Disney+, etc.)
- `price` (1-4) → `subscriptionTier` (1-3: Free/Standard/Premium)
- Add: `director` (string)
- Add: `releaseYear` (number)
- Keep: `photo` (for movie posters), `avgRating`, `numRatings`, `sumRating`, `timestamp`

### Subcollection: ratings (unchanged structure)

Reviews/ratings remain the same structure but context changes to movie reviews.

## File Changes

### 1. Data Generation Files

**`src/lib/randomData.js`**

- Replace `restaurantNames` with `movieTitles` (50+ movie titles)
- Replace `restaurantCities` with `streamingServices` (Netflix, Disney+, Hulu, Prime Video, HBO Max, Apple TV+, Paramount+, Peacock)
- Replace `restaurantCategories` with `movieGenres` (Action, Comedy, Drama, Sci-Fi, Horror, Romance, Thriller, Documentary, Animation, Fantasy)
- Update `restaurantReviews` to `movieReviews` (adjust review text to be movie-appropriate)
- Add `directors` array (famous directors)
- Add `releaseYears` array (2000-2024)

**`src/lib/fakeRestaurants.js`**

- Rename to `src/lib/fakeMovies.js`
- Update `generateFakeRestaurantsAndReviews()` → `generateFakeMoviesAndReviews()`
- Update field mappings to use new movie fields
- Change price range from 1-4 to 1-3 for subscription tiers
- Add director and releaseYear to generated data
- Update photo URLs to use movie poster placeholders

### 2. Firestore Functions

**`src/lib/firebase/firestore.js`**

- Update all function names: `Restaurant` → `Movie`
- `getRestaurants` → `getMovies`
- `getRestaurantById` → `getMovieById`
- `getRestaurantSnapshotById` → `getMovieSnapshotById`
- `getRestaurantsSnapshot` → `getMoviesSnapshot`
- `updateRestaurantImageReference` → `updateMovieImageReference`
- `addReviewToRestaurant` → `addReviewToMovie`
- `addFakeRestaurantsAndReviews` → `addFakeMoviesAndReviews`
- Update collection references: `"restaurants"` → `"movies"`
- Update `applyQueryFilters` to handle:
- `genre` instead of `category`
- `streamingService` instead of `city`
- `subscriptionTier` instead of `price` (1-3 range)
- Add `releaseYear` filter
- Add `director` filter

### 3. Storage Functions

**`src/lib/firebase/storage.js`**

- Rename `updateRestaurantImage` → `updateMovieImage`
- Update storage path: `images/${restaurantId}` → `images/${movieId}`
- Update function parameters and comments

### 4. Component Files

**`src/components/Restaurant.jsx`**

- Rename to `src/components/Movie.jsx`
- Update all variable names: `restaurant` → `movie`
- Update function: `handleRestaurantImage` → `handleMoviePoster`
- Update imports and function calls

**`src/components/RestaurantDetails.jsx`**

- Rename to `src/components/MovieDetails.jsx`
- Update props and display:
- `restaurant.name` → `movie.title`
- Display: `{movie.genre} | {movie.streamingService}`
- Replace price display with subscription tier: `Free`, `Standard`, or `Premium`
- Add display for `director` and `releaseYear`
- Update image alt text references

**`src/components/RestaurantListings.jsx`**

- Rename to `src/components/MovieListings.jsx`
- Update component names:
- `RestaurantItem` → `MovieItem`
- `ActiveResturant` → `ActiveMovie`
- `ResturantDetails` → `MovieDetails`
- `RestaurantRating` → `MovieRating`
- `RestaurantMetadata` → `MovieMetadata`
- Update all variable references
- Update metadata display to show genre and streaming service

**`src/components/Filters.jsx`**

- Update filter options:
- Category → Genre (Action, Comedy, Drama, Sci-Fi, Horror, Romance, Thriller, Documentary, Animation, Fantasy)
- City → Streaming Service (Netflix, Disney+, Hulu, Prime Video, HBO Max, Apple TV+, Paramount+, Peacock)
- Price → Subscription Tier (Free, Standard, Premium)
- Keep Sort (Rating, Review)
- Add Release Year filter (2000-2024 range or dropdown)
- Update labels and display text
- Update filter summary text: "Restaurants" → "Movies"

**`src/components/ReviewDialog.jsx`**

- Update context references from restaurant to movie
- Update any displayed text

### 5. Page Components

**`src/app/page.js`**

- Update imports: `RestaurantListings` → `MovieListings`
- Update function calls: `getRestaurants` → `getMovies`
- Update variable names: `restaurants` → `movies`
- Update props: `initialRestaurants` → `initialMovies`

**`src/app/restaurant/[id]/page.jsx`**

- Rename directory: `restaurant` → `movie`
- Update imports and component references
- Update function calls: `getRestaurantById` → `getMovieById`
- Update variable: `restaurant` → `movie`
- Update props passed to components

**`src/app/restaurant/[id]/error.jsx`**

- Move to `src/app/movie/[id]/error.jsx`
- Update error messages if they reference restaurants

### 6. Layout and Branding

**`src/app/layout.js`**

- Update metadata:
- `title`: "FriendlyEats" → "MovieReviews" or "CineRate"
- `description`: Update to reflect movie review site

**`src/components/Header.jsx`**

- Update logo text: "Friendly Eats" → "CineRate" (or similar)
- Update menu text: "Add sample restaurants" → "Add sample movies"
- Update function call: `addFakeRestaurantsAndReviews` → `addFakeMoviesAndReviews`

**`readme.md`**

- Update title and description
- Update instructions to reflect movie review context

### 7. Actions

**`src/app/actions.js`**

- Update any server actions that reference restaurants

## Testing Checklist

- Verify movie listings display with correct fields
- Test filtering by genre, streaming service, subscription tier, release year
- Test sorting by rating and review count
- Test adding reviews to movies
- Test uploading movie posters
- Test adding sample movies via menu
- Verify Firestore collection name changed to `movies`
- Verify all UI text updated from restaurant to movie terminology

### To-dos

- [ ] Update data generation files (randomData.js, fakeRestaurants.js → fakeMovies.js) with movie-specific data
- [ ] Update Firestore functions to use movies collection and new field names
- [ ] Update storage functions for movie posters
- [ ] Rename and update all components (Restaurant → Movie, RestaurantDetails → MovieDetails, etc.)
- [ ] Update Filters component with movie-specific filters (Genre, Streaming Service, Subscription Tier, Release Year)
- [ ] Update page components and rename restaurant route to movie route
- [ ] Update layout, header, and branding from FriendlyEats to movie review site