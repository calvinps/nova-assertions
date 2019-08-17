# Nova Assertions

[![Latest Version on Github](https://img.shields.io/github/release/dillingham/nova-assertions.svg?style=flat-square)](https://packagist.org/packages/dillingham/nova-assertions)
[![Total Downloads](https://img.shields.io/packagist/dt/dillingham/nova-assertions.svg?style=flat-square)](https://packagist.org/packages/dillingham/nova-assertions)

Nova requests & assertions for Laravel tests

Quick links:
[Policies](https://github.com/dillingham/nova-assertions#assert-policies) |
[Cards](https://github.com/dillingham/nova-assertions#assert-cards) |
[Actions](https://github.com/dillingham/nova-assertions#assert-actions) |
[Filters](https://github.com/dillingham/nova-assertions#assert-filters) |
[Lenses](https://github.com/dillingham/nova-assertions#assert-lenses) |
[Resources](https://github.com/dillingham/nova-assertions#assert-resources) |
[Fields](https://github.com/dillingham/nova-assertions#assert-fields)

### Installation

```
composer require dillingham/nova-assertions
```
```php
use NovaTesting\NovaAssertions;

class UserTest extends TestCase
{
    use NovaAssertions;
}
```
Enable by adding the `NovaAssertions` to a test

### Usage Example
```php
public function testNova()
{
    $this->be(factory(User::class)->create());

    $this->novaIndex('users')
        ->assertCanUpdate()
        ->assertCannotDelete()
        ->assertFieldsInclude('email')
        ->assertFieldsExclude('password');
}
```

### Authentication
Log in a user that **[has access to Nova](https://nova.laravel.com/docs/2.0/installation.html#authorizing-nova)**
```php
$this->be(factory(User::class)->create());
```

### Nova Requests

Request Nova's results with one of the following:

```php
$response = $this->novaIndex('users');
```
```php
$response = $this->novaDetail('users', $user->id);
```
```php
$response = $this->novaCreate('users');
```
```php
$response = $this->novaEdit('users', $user->id);
```
```php
$response = $this->novaLens('users', Lens::class);
```

### Request Filters
You may also pass a filter & it's value to indexes & lenses
```php
$response = $this->novaIndex('users', [
    StatusFilter::class => 'active'
]);
```
```php
$response = $this->novaLens('users', Lens::class, [
    StatusFilter::class => 'active'
]);
```
### Assert Http
You can call **[http response methods](https://laravel.com/docs/5.8/http-tests#available-assertions)** as usual:

```php
$response->assertOk();
```

### Assert Policies

The following assert against the auth user & **[Nova's use of policies](https://nova.laravel.com/docs/2.0/resources/authorization.html#authorization)**

```php
$response->assertCanView();
```
```php
$response->assertCanCreate();
```
```php
$response->assertCanUpdate();
```
```php
$response->assertCanDelete();
```
```php
$response->assertCanForceDelete();
```
```php
$response->assertCanRestore();
```
```php
```
Also able to assert `cannot` for each:
```php
$response->assertCannotView();
```

### Assert Cards
```php
$response->assertCardCount(5);
```
```php
$response->assertCardsInclude(Card::class);
```
```php
$response->assertCardsExclude(Card::class);
```
```php
$response->assertCards(function($cards) {
    return $cards->count() > 0;
});
```

### Assert Actions
```php
$response->assertActionCount(5);
```
```php
$response->assertActionsInclude(Action::class);
```
```php
$response->assertActionsExclude(Action::class);
```
```php
$response->assertActions(function($actions) {
    return $actions->count() > 0;
});
```
### Assert Filters
```php
$response->assertFilterCount(5);
```
```php
$response->assertFiltersInclude(Filter::class);
```
```php
$response->assertFiltersExclude(Filter::class);
```
```php
$response->assertFilters(function($filters) {
    return $filters->count() > 0;
});
```
### Assert Lenses
```php
$response->assertLensCount(5);
```
```php
$response->assertLensesInclude(Lens::class);
```
```php
$response->assertLensesExclude(Lens::class);
```
```php
$response->assertLenses(function($lenses) {
    return $lenses->count() > 0;
});
```
### Assert Resources
```php
$response->assertResourceCount(3);
```
```php
$response->assertResources(function($resources) {
    return $resources->count() > 0;
});
```
### Assert Fields
```php
$response->assertFieldCount(5);
```
Assert a specific field exists
```php
$response->assertFieldsInclude('id');
```
Assert a specific field contains a value
```php
$response->assertFieldsInclude('id', $user->id);
```
Assert multiple fields exist
```php
$response->assertFieldsInclude(['id', 'email']);
```
Assert multiple fields with specific values exist
```php
$response->assertFieldsInclude(['id' => 1, 'email' => 'example']);
```
Assert multiple values for one field exist
```php
$response->assertFieldsInclude('id', $users->pluck('id'));
```
Make assertions against a collection of fields
```php
$response->assertFields(function($fields) {
    return $fields->count() > 0;
});
```
Also `exclude` works in all of these scenarios
```php
$response->assertFieldsExclude(['id' => 1, 'email' => 'example']);
```
