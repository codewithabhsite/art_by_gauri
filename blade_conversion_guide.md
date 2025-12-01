# Blade Template Conversion Examples

## 1. Main Layout (app.blade.php)

```blade
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@yield('title', 'Art by Gauri')</title>
    <meta name="description" content="@yield('description', 'Professional charcoal artist and art teacher')">
    
    @vite(['resources/css/app.css', 'resources/js/app.js'])
    
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Playfair+Display:wght@600;700&display=swap" rel="stylesheet">
</head>
<body class="bg-zinc-900 text-gray-100">
    
    @include('components.header')
    
    <main>
        @yield('content')
    </main>
    
    @include('components.footer')
    
    @stack('scripts')
</body>
</html>
```

---

## 2. Header Component (components/header.blade.php)

```blade
<header class="bg-black/50 backdrop-blur-md fixed w-full z-50 border-b border-zinc-800">
    <nav class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center h-16">
            <div class="flex-shrink-0">
                <a href="{{ route('home') }}" class="text-2xl font-display font-bold text-cream">Art by Gauri</a>
            </div>
            <div class="hidden md:flex items-center space-x-8">
                <a href="{{ route('home') }}" class="text-gray-300 hover:text-gold transition {{ request()->routeIs('home') ? 'text-cream' : '' }}">Home</a>
                <a href="{{ route('portfolio') }}" class="text-gray-300 hover:text-gold transition {{ request()->routeIs('portfolio') ? 'text-cream' : '' }}">Portfolio</a>
                <a href="{{ route('services') }}" class="text-gray-300 hover:text-gold transition {{ request()->routeIs('services') ? 'text-cream' : '' }}">Services</a>
                <a href="{{ route('classes.index') }}" class="text-gray-300 hover:text-gold transition {{ request()->routeIs('classes.*') ? 'text-cream' : '' }}">Classes</a>
                <a href="{{ route('about') }}" class="text-gray-300 hover:text-gold transition {{ request()->routeIs('about') ? 'text-cream' : '' }}">About</a>
                <a href="{{ route('contact') }}" class="text-gray-300 hover:text-gold transition {{ request()->routeIs('contact') ? 'text-cream' : '' }}">Contact</a>
            </div>
            <div class="hidden lg:flex items-center space-x-4">
                @auth
                    <a href="{{ route('dashboard') }}" class="px-4 py-2 border border-cream text-cream font-medium rounded-lg hover:bg-cream/10 transition">Dashboard</a>
                @else
                    <a href="{{ route('login') }}" class="px-4 py-2 border border-cream text-cream font-medium rounded-lg hover:bg-cream/10 transition">Login</a>
                @endauth
                <a href="{{ route('booking') }}" class="px-4 py-2 bg-gold text-black font-medium rounded-lg hover:bg-gold/90 transition">Book Portrait</a>
            </div>
            <button id="mobile-menu-btn" class="md:hidden text-cream">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
                </svg>
            </button>
        </div>
    </nav>
    <!-- Mobile menu -->
    <div id="mobile-menu" class="hidden md:hidden bg-black/95 border-t border-zinc-800">
        <div class="px-4 pt-2 pb-4 space-y-2">
            <a href="{{ route('home') }}" class="block px-3 py-2 text-gray-300 hover:bg-zinc-800 rounded">Home</a>
            <a href="{{ route('portfolio') }}" class="block px-3 py-2 text-gray-300 hover:bg-zinc-800 rounded">Portfolio</a>
            <a href="{{ route('services') }}" class="block px-3 py-2 text-gray-300 hover:bg-zinc-800 rounded">Services</a>
            <a href="{{ route('classes.index') }}" class="block px-3 py-2 text-gray-300 hover:bg-zinc-800 rounded">Classes</a>
            <a href="{{ route('about') }}" class="block px-3 py-2 text-gray-300 hover:bg-zinc-800 rounded">About</a>
            <a href="{{ route('contact') }}" class="block px-3 py-2 text-gray-300 hover:bg-zinc-800 rounded">Contact</a>
        </div>
    </div>
</header>
```

---

## 3. Footer Component (components/footer.blade.php)

```blade
<footer class="bg-black border-t border-zinc-800 py-12">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="grid grid-cols-1 md:grid-cols-4 gap-8 mb-8">
            <div>
                <h3 class="text-2xl font-display font-bold text-cream mb-4">Art by Gauri</h3>
                <p class="text-gray-400 text-sm">Professional charcoal artist and art teacher from Shahjahanpur, India.</p>
            </div>
            <div>
                <h4 class="font-semibold text-cream mb-4">Quick Links</h4>
                <ul class="space-y-2 text-sm">
                    <li><a href="{{ route('home') }}" class="text-gray-400 hover:text-gold transition">Home</a></li>
                    <li><a href="{{ route('portfolio') }}" class="text-gray-400 hover:text-gold transition">Portfolio</a></li>
                    <li><a href="{{ route('services') }}" class="text-gray-400 hover:text-gold transition">Services</a></li>
                    <li><a href="{{ route('classes.index') }}" class="text-gray-400 hover:text-gold transition">Classes</a></li>
                </ul>
            </div>
            <div>
                <h4 class="font-semibold text-cream mb-4">More</h4>
                <ul class="space-y-2 text-sm">
                    <li><a href="{{ route('about') }}" class="text-gray-400 hover:text-gold transition">About</a></li>
                    <li><a href="{{ route('contact') }}" class="text-gray-400 hover:text-gold transition">Contact</a></li>
                    <li><a href="{{ route('booking') }}" class="text-gray-400 hover:text-gold transition">Book Portrait</a></li>
                </ul>
            </div>
            <div>
                <h4 class="font-semibold text-cream mb-4">Connect</h4>
                <div class="space-y-3">
                    <a href="#" class="flex items-center text-gray-400 hover:text-gold transition text-sm">
                        <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 24 24"><!-- Instagram icon --></svg>
                        Instagram
                    </a>
                    <a href="#" class="flex items-center text-gray-400 hover:text-gold transition text-sm">
                        <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 24 24"><!-- WhatsApp icon --></svg>
                        WhatsApp
                    </a>
                    <p class="text-gray-400 text-sm mt-4">
                        📍 Shahjahanpur, Uttar Pradesh, India
                    </p>
                </div>
            </div>
        </div>
        <div class="border-t border-zinc-800 pt-8 text-center text-gray-400 text-sm">
            <p>&copy; {{ date('Y') }} Art by Gauri. All rights reserved.</p>
        </div>
    </div>
</footer>
```

---

## 4. Example Page Conversion (home.blade.php)

```blade
@extends('layouts.app')

@section('title', 'Home - Art by Gauri')
@section('description', 'Professional charcoal portraits and art classes by Gauri')

@section('content')
<!-- Hero Section -->
<section class="pt-24 pb-20 bg-gradient-to-br from-zinc-900 via-zinc-800 to-black">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="text-center max-w-4xl mx-auto">
            <h1 class="text-5xl sm:text-6xl lg:text-7xl font-display font-bold text-cream mb-6 leading-tight">
                Timeless Charcoal <span class="text-gold">Portraits</span>
            </h1>
            <p class="text-xl sm:text-2xl text-gray-300 mb-4">
                Capturing emotions, preserving memories
            </p>
            <p class="text-gray-400 mb-8">
                ✨ 2+ years of experience • 50+ happy clients • Personalized art classes
            </p>
            <div class="flex flex-col sm:flex-row gap-4 justify-center">
                <a href="{{ route('booking') }}" class="px-8 py-4 bg-gold text-black text-lg font-semibold rounded-lg hover:bg-gold/90 transition transform hover:scale-105">
                    Book a Portrait
                </a>
                <a href="{{ route('portfolio') }}" class="px-8 py-4 border-2 border-cream text-cream text-lg font-semibold rounded-lg hover:bg-cream/10 transition">
                    View Portfolio
                </a>
                <a href="{{ route('classes.index') }}" class="px-8 py-4 border-2 border-gold text-gold text-lg font-semibold rounded-lg hover:bg-gold/10 transition">
                    Join a Class
                </a>
            </div>
        </div>
    </div>
</section>

<!-- Featured Artworks -->
<section class="py-20 bg-zinc-900">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-12">
            <h2 class="text-4xl font-display font-bold text-cream mb-4">Featured Artworks</h2>
            <p class="text-gray-400">Explore my latest charcoal creations</p>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            @foreach($featuredArtworks as $artwork)
                @include('components.artwork-card', ['artwork' => $artwork])
            @endforeach
        </div>
        
        <div class="text-center mt-12">
            <a href="{{ route('portfolio') }}" class="inline-block px-8 py-3 border-2 border-gold text-gold font-semibold rounded-lg hover:bg-gold/10 transition">
                View Full Portfolio →
            </a>
        </div>
    </div>
</section>

<!-- More sections... -->
@endsection

@push('scripts')
<script>
    // Page-specific JavaScript
    console.log('Home page loaded');
</script>
@endpush
```

---

## 5. Artwork Card Component (components/artwork-card.blade.php)

```blade
<div class="artwork-card group relative bg-zinc-800 rounded-xl overflow-hidden border border-zinc-700 hover:border-gold transition">
    <div class="aspect-square bg-zinc-700 overflow-hidden">
        @if($artwork->image_path)
            <img src="{{ asset('storage/' . $artwork->image_path) }}" alt="{{ $artwork->title }}" class="w-full h-full object-cover group-hover:scale-110 transition duration-300">
        @else
            <div class="w-full h-full flex items-center justify-center">
                <span class="text-6xl">🖼️</span>
            </div>
        @endif
    </div>
    
    <div class="p-4">
        <div class="flex items-center justify-between mb-2">
            <h3 class="text-lg font-semibold text-cream">{{ $artwork->title }}</h3>
            @if($artwork->status === 'available')
                <span class="px-2 py-1 bg-green-500/20 text-green-400 text-xs font-semibold rounded-full">Available</span>
            @else
                <span class="px-2 py-1 bg-red-500/20 text-red-400 text-xs font-semibold rounded-full">Sold</span>
            @endif
        </div>
        <p class="text-sm text-gray-400">{{ ucfirst($artwork->category) }}</p>
    </div>
    
    <!-- Hover overlay -->
    <div class="absolute inset-0 bg-black/80 opacity-0 group-hover:opacity-100 transition flex items-center justify-center">
        <a href="{{ route('artworks.show', $artwork) }}" class="px-6 py-3 bg-gold text-black font-semibold rounded-lg hover:bg-gold/90 transition">
            View Details
        </a>
    </div>
</div>
```

---

## 6. Routes Example (web.php)

```php
<?php

use App\Http\Controllers\HomeController;
use App\Http\Controllers\PortfolioController;
use App\Http\Controllers\ServiceController;
use App\Http\Controllers\ClassController;
use App\Http\Controllers\BookingController;
use App\Http\Controllers\ContactController;
use App\Http\Controllers\DashboardController;
use App\Http\Controllers\Admin;
use Illuminate\Support\Facades\Route;

// Public routes
Route::get('/', [HomeController::class, 'index'])->name('home');
Route::get('/portfolio', [PortfolioController::class, 'index'])->name('portfolio');
Route::get('/services', [ServiceController::class, 'index'])->name('services');
Route::get('/about', [HomeController::class, 'about'])->name('about');

// Classes
Route::get('/classes', [ClassController::class, 'index'])->name('classes.index');
Route::get('/classes/{class}', [ClassController::class, 'show'])->name('classes.show');

// Booking
Route::get('/booking', [BookingController::class, 'index'])->name('booking');
Route::post('/booking', [BookingController::class, 'store'])->name('booking.store');

// Contact
Route::get('/contact', [ContactController::class, 'index'])->name('contact');
Route::post('/contact', [ContactController::class, 'send'])->name('contact.send');

// User dashboard (authenticated)
Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index'])->name('dashboard');
    Route::get('/dashboard/orders', [DashboardController::class, 'orders'])->name('dashboard.orders');
    Route::get('/dashboard/classes', [DashboardController::class, 'classes'])->name('dashboard.classes');
    Route::get('/dashboard/profile', [DashboardController::class, 'profile'])->name('dashboard.profile');
    Route::put('/dashboard/profile', [DashboardController::class, 'updateProfile'])->name('dashboard.profile.update');
});

// Admin routes (admin only)
Route::middleware(['auth', 'admin'])->prefix('admin')->name('admin.')->group(function () {
    Route::get('/dashboard', [Admin\DashboardController::class, 'index'])->name('dashboard');
    
    // Artworks
    Route::resource('artworks', Admin\ArtworkController::class);
    
    // Orders
    Route::resource('orders', Admin\OrderController::class);
    Route::put('orders/{order}/status', [Admin\OrderController::class, 'updateStatus'])->name('orders.status');
    
    // Classes
    Route::resource('classes', Admin\ClassController::class);
    
    // Payments
    Route::get('payments', [Admin\PaymentController::class, 'index'])->name('payments.index');
});

require __DIR__.'/auth.php';
```

---

## 7. Controller Example (HomeController.php)

```php
<?php

namespace App\Http\Controllers;

use App\Models\Artwork;
use App\Models\Testimonial;
use Illuminate\Http\Request;

class HomeController extends Controller
{
    public function index()
    {
        $featuredArtworks = Artwork::where('is_featured', true)
            ->where('status', 'available')
            ->latest()
            ->take(6)
            ->get();
            
        $testimonials = Testimonial::where('is_featured', true)
            ->latest()
            ->take(3)
            ->get();
            
        return view('pages.home', compact('featuredArtworks', 'testimonials'));
    }
    
    public function about()
    {
        return view('pages.about');
    }
}
```

---

## 8. Model Example (Artwork.php)

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Artwork extends Model
{
    use HasFactory;
    
    protected $fillable = [
        'title',
        'description',
        'image_path',
        'category',
        'status',
        'medium',
        'size',
        'is_featured',
    ];
    
    protected $casts = [
        'is_featured' => 'boolean',
    ];
    
    // Scopes
    public function scopeFeatured($query)
    {
        return $query->where('is_featured', true);
    }
    
    public function scopeAvailable($query)
    {
        return $query->where('status', 'available');
    }
    
    public function scopeByCategory($query, $category)
    {
        return $query->where('category', $category);
    }
}
```

---

## Conversion Checklist

### For Each HTML Page:

1. **Create Blade file** in appropriate directory
2. **Add @extends directive** at top
3. **Add @section('title')** and **@section('description')**
4. **Wrap content** in `@section('content')`
5. **Replace static links**:
   - `href="home.html"` → `href="{{ route('home') }}"`
   - `href="portfolio.html"` → `href="{{ route('portfolio') }}"`
6. **Replace asset paths**:
   - `src="/images/photo.jpg"` → `src="{{ asset('images/photo.jpg') }}"`
7. **Add dynamic data** where needed:
   - Static artwork cards → `@foreach($artworks as $artwork)`
8. **Move JavaScript** to `@push('scripts')` section
9. **Test the page** in browser

### Common Blade Directives:

- `@extends('layout')` - Extend a layout
- `@section('name')` - Define a section
- `@yield('name')` - Output a section
- `@include('view')` - Include a partial
- `@if`, `@else`, `@endif` - Conditionals
- `@foreach`, `@endforeach` - Loops
- `@auth`, `@guest` - Authentication checks
- `{{ $variable }}` - Echo escaped data
- `{!! $html !!}` - Echo unescaped HTML
- `@push('scripts')` - Push to stack
- `@stack('scripts')` - Output stack

---

Ready to convert your HTML to Laravel Blade templates!
