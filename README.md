# Fonty
Typeface for all views in Android

How to implement?

Create a new class named Fonty
and add this code:

    private AssetManager mAssetManager;
    private Map<String, Typeface> mTypefaceMap;
	
	public Fonty(Context context){
		mTypefaceMap = new HashMap<>();
		mAssetManager = context.getAssets();
	}
	
	public void setFont(Activity activity, String fontPath, boolean includeActionbar) {
        Typeface typeface = cacheFont(fontPath);
        View rootView = includeActionbar ? activity.getWindow().getDecorView()
		: ((ViewGroup) activity.findViewById(android.R.id.content)).getChildAt(0);
        traverseView(rootView, typeface);
    }
	
	public void setFont(View view, String fontPath) {
        Typeface typeface = cacheFont(fontPath);
        traverseView(view, typeface);
    }
	
	private void traverseView(View view, Typeface typeface) {
        if (view instanceof ViewGroup) {
            ViewGroup viewGroup = (ViewGroup) view;
            for (int i = 0; i < viewGroup.getChildCount(); i++) {
                View v = viewGroup.getChildAt(i);
                if (v instanceof TextView) {
                    ((TextView) v).setTypeface(typeface);
                }
                if (v instanceof ViewGroup) {
                    traverseView(v, typeface);
                }
            }
        }
	}
	
	private Typeface cacheFont(String fontPath) {
        Typeface cached = mTypefaceMap.get(fontPath);
        if (cached == null) {
            cached = Typeface.createFromAsset(mAssetManager, fontPath);
            mTypefaceMap.put(fontPath, cached);
        }
        return cached;
    }

How to use ?

In the activity you want to use Fonty add this code at the end of onCreate method:
    
    Fonty fonty = new Fonty(getApplicationContext());
		fonty.setFont(this, "productsans/ProductSans-Regular.ttf", true);

setFont(Activity activity, String fontPath boolean includeActionBar);

For custom views:

    Fonty fonty = new Fonty(getApplicationContext());
		fonty.setFont(view, "productsans/ProductSans-Regular.ttf");
    
setFont(View view, String fontPath);
