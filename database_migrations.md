# Database Migrations - Art by Gauri

## Migration Files to Create

Run these commands in order after Laravel is set up:

```bash
php artisan make:migration create_artworks_table
php artisan make:migration create_orders_table
php artisan make:migration create_classes_table
php artisan make:migration create_enrollments_table
php artisan make:migration create_payments_table
php artisan make:migration create_testimonials_table
php artisan make:migration add_role_to_users_table
```

---

## 1. Artworks Table

**File:** `database/migrations/xxxx_create_artworks_table.php`

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('artworks', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('description')->nullable();
            $table->string('image_path');
            $table->enum('category', ['portraits', 'couples', 'kids', 'concept']);
            $table->enum('status', ['available', 'sold'])->default('available');
            $table->string('medium')->default('Charcoal on paper');
            $table->string('size')->nullable();
            $table->boolean('is_featured')->default(false);
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('artworks');
    }
};
```

---

## 2. Orders Table

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('orders', function (Blueprint $table) {
            $table->id();
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->string('order_number')->unique();
            $table->enum('service_type', ['single', 'couple', 'family']);
            $table->enum('paper_size', ['a4', 'a3', 'custom']);
            $table->string('custom_size')->nullable();
            $table->string('occasion')->nullable();
            $table->text('special_instructions')->nullable();
            $table->json('reference_photos')->nullable();
            $table->enum('status', ['pending', 'in_progress', 'completed', 'cancelled'])->default('pending');
            $table->decimal('amount', 10, 2);
            $table->decimal('advance_paid', 10, 2)->default(0);
            $table->decimal('balance', 10, 2);
            $table->date('estimated_delivery')->nullable();
            $table->date('actual_delivery')->nullable();
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('orders');
    }
};
```

---

## 3. Classes Table

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('classes', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('description');
            $table->enum('mode', ['online', 'offline']);
            $table->integer('duration_weeks');
            $table->integer('total_sessions');
            $table->string('schedule'); // e.g., "Saturdays & Sundays, 10 AM - 12 PM"
            $table->decimal('fee', 10, 2);
            $table->integer('max_students');
            $table->integer('enrolled_students')->default(0);
            $table->enum('status', ['open', 'full', 'closed'])->default('open');
            $table->text('curriculum')->nullable();
            $table->date('start_date')->nullable();
            $table->date('end_date')->nullable();
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('classes');
    }
};
```

---

## 4. Enrollments Table

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('enrollments', function (Blueprint $table) {
            $table->id();
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->foreignId('class_id')->constrained()->onDelete('cascade');
            $table->enum('status', ['active', 'completed', 'cancelled'])->default('active');
            $table->integer('current_week')->default(1);
            $table->decimal('amount_paid', 10, 2);
            $table->date('enrolled_at');
            $table->timestamps();
            
            // Prevent duplicate enrollments
            $table->unique(['user_id', 'class_id']);
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('enrollments');
    }
};
```

---

## 5. Payments Table

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('payments', function (Blueprint $table) {
            $table->id();
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->morphs('payable'); // Can be order_id or enrollment_id
            $table->string('transaction_id')->unique();
            $table->decimal('amount', 10, 2);
            $table->enum('status', ['pending', 'paid', 'failed', 'refunded'])->default('pending');
            $table->string('payment_method')->nullable(); // razorpay, cash, etc.
            $table->json('payment_details')->nullable(); // Store Razorpay response
            $table->timestamp('paid_at')->nullable();
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('payments');
    }
};
```

---

## 6. Testimonials Table

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('testimonials', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->enum('type', ['client', 'student']);
            $table->text('quote');
            $table->integer('rating')->default(5);
            $table->boolean('is_featured')->default(false);
            $table->string('image')->nullable();
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('testimonials');
    }
};
```

---

## 7. Add Role to Users Table

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::table('users', function (Blueprint $table) {
            $table->enum('role', ['user', 'admin'])->default('user')->after('email');
            $table->string('phone')->nullable()->after('email');
            $table->string('city')->nullable()->after('phone');
            $table->string('profile_photo')->nullable()->after('city');
        });
    }

    public function down(): void
    {
        Schema::table('users', function (Blueprint $table) {
            $table->dropColumn(['role', 'phone', 'city', 'profile_photo']);
        });
    }
};
```

---

## Running Migrations

```bash
# Run all migrations
php artisan migrate

# Rollback last migration
php artisan migrate:rollback

# Rollback all and re-run
php artisan migrate:fresh

# Rollback all, re-run, and seed
php artisan migrate:fresh --seed
```

---

## Database Seeders (Optional)

### Create Seeders

```bash
php artisan make:seeder ArtworkSeeder
php artisan make:seeder TestimonialSeeder
php artisan make:seeder AdminUserSeeder
```

### Admin User Seeder Example

**File:** `database/seeders/AdminUserSeeder.php`

```php
<?php

namespace Database\Seeders;

use App\Models\User;
use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\Hash;

class AdminUserSeeder extends Seeder
{
    public function run(): void
    {
        User::create([
            'name' => 'Gauri',
            'email' => 'admin@artbygauri.com',
            'password' => Hash::make('password'), // Change this!
            'role' => 'admin',
            'phone' => '+91 9876543210',
            'city' => 'Shahjahanpur',
            'email_verified_at' => now(),
        ]);
    }
}
```

### Run Seeders

```bash
php artisan db:seed
# or
php artisan db:seed --class=AdminUserSeeder
```

---

## Model Relationships

### User Model

```php
public function orders()
{
    return $this->hasMany(Order::class);
}

public function enrollments()
{
    return $this->hasMany(Enrollment::class);
}

public function payments()
{
    return $this->hasMany(Payment::class);
}

public function isAdmin()
{
    return $this->role === 'admin';
}
```

### Order Model

```php
public function user()
{
    return $this->belongsTo(User::class);
}

public function payments()
{
    return $this->morphMany(Payment::class, 'payable');
}
```

### Class Model

```php
public function enrollments()
{
    return $this->hasMany(Enrollment::class);
}

public function students()
{
    return $this->belongsToMany(User::class, 'enrollments')
        ->withPivot('status', 'current_week', 'amount_paid')
        ->withTimestamps();
}
```

### Enrollment Model

```php
public function user()
{
    return $this->belongsTo(User::class);
}

public function class()
{
    return $this->belongsTo(ClassModel::class, 'class_id');
}

public function payments()
{
    return $this->morphMany(Payment::class, 'payable');
}
```

---

## Middleware for Admin

### Create Middleware

```bash
php artisan make:middleware EnsureUserIsAdmin
```

**File:** `app/Http/Middleware/EnsureUserIsAdmin.php`

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;

class EnsureUserIsAdmin
{
    public function handle(Request $request, Closure $next): Response
    {
        if (!auth()->check() || !auth()->user()->isAdmin()) {
            abort(403, 'Unauthorized action.');
        }

        return $next($request);
    }
}
```

### Register Middleware

**File:** `app/Http/Kernel.php` or `bootstrap/app.php` (Laravel 11+)

```php
protected $middlewareAliases = [
    // ...
    'admin' => \App\Http\Middleware\EnsureUserIsAdmin::class,
];
```

---

Ready to run migrations once Laravel is set up!
