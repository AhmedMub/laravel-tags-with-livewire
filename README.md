
# Use spatie/laravel-tags with Livewire


For more information on the package, please refer to the [Spatie documentation](https://spatie.be/docs/laravel-tags), as below explanation for those already familiar with it.

In order to provide the explanations below, I spent many hours researching how to use the package with Livewire, so if it helped you, please give it a star. If not, please let me know how to improve it.

After making an Eloquent model taggable by adding the \Spatie\Tags\HasTags trait to it::

```php
// apply HasTags trait to a model
use Illuminate\Database\Eloquent\Model;
use Spatie\Tags\HasTags;

class NewsItem extends Model
{
    use HasTags;
    
    // ...
}
```

```php

// In your livewire component, you should use model binding and set the wire model:
<input wire:model='name.en' type="text" />

//And for translation in case, you have another input for multi-language functionality
<input wire:model='name.ar' type="text" />

// to use livewire validation
protected $rules = [
        'name.en' => ['required', 'string'],
        'name.ar' => ['required', 'string'],
    ];

protected $messages = [
    'name.en.required' => 'This field is required',
    'name.ar.required' => 'This field is required',
];

// here to save the tag with translation if you have any
$tag = new Tag;
$tag->setTranslation('name', 'en', $this->name['en'])
    ->setTranslation('name', 'ar', $this->name['ar'])
    ->save();

// this is to update the tag, already explained in the documentation
$tag = Tag::findOrFail($id);
$tag->setTranslation('name', 'en', $this->name['en']);
$tag->setTranslation('name', 'ar', $this->name['ar']);
$tag->save();

// to display all the tags with any selected translation that exists
@foreach ($tags as $tag)
    <div> {{$tag->getTranslation('name', 'en')}} </div>
    <div> {{$tag->getTranslation('name', 'ar')}} </div>
@endforeach
```

