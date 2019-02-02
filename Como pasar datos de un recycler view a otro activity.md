Como puedo hacer para pasar los datos de mi recicler view a otro activity
quiero que cuando clickee sobre un item me mande la informacion de ese item a otro activity junto a su ID de Firebasepara que en la otra actividad yo pueda manupular su informacion ya sea editarla o borrarla

esta seria la clase principal

public class Buscarservicios extends AppCompatActivity  {

    RecyclerView rv;
    List<Servicios>serviciosList;
   adapter adapter;
   FirebaseDatabase firebaseDatabase;
   DatabaseReference databaseReference;
   Adapter mFirebaseAdapter;
   FirebaseDatabase database;



   @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_buscarservicios);

        rv = (RecyclerView) findViewById(R.id.recicler);
        rv.setLayoutManager(new LinearLayoutManager(this));


         serviciosList = new ArrayList<>();
        try{
        firebaseDatabase = FirebaseDatabase.getInstance();

        }catch(Exception ex){Log.d("asd",""+ex);}


         adapter = new adapter(serviciosList);

         adapter.setOnclickListenes(new View.OnClickListener() {
             @Override
             public void onClick(View v) {

                 Toast.makeText(Buscarservicios.this, "clikeado" , Toast.LENGTH_LONG).show();

                 Intent intent=new Intent(Buscarservicios.this,ConfirmarServicioActivity.class);
               // intent.putExtra("id",adapter.getItemId());
                 startActivity(intent);
             }
         });

         rv.setAdapter(adapter);

        DatabaseReference databasereference = firebaseDatabase.getReference("Komchen/Servicios");
        databasereference.addValueEventListener(new ValueEventListener() {
             @Override
             public void onDataChange(@NonNull DataSnapshot dataSnapshot) {


                     serviciosList.removeAll(serviciosList);
                     for (DataSnapshot snapshot : dataSnapshot.getChildren ()){


                         Servicios servicios =  snapshot.getValue(Servicios.class);
                         serviciosList.add(servicios);
                         Log.d("asd",servicios+"|"+dataSnapshot.getValue().toString());

                     }
                     adapter.notifyDataSetChanged();

             }
             @Override
             public void onCancelled(@NonNull DatabaseError databaseError) {
                 Toast.makeText(Buscarservicios.this ,"database error"+databaseError, Toast.LENGTH_SHORT).show();

             }
         });


    }
}


Este el adaptador


public  class adapter extends RecyclerView.Adapter<adapter.Serviciosviewholder>
implements View.OnClickListener {

    List<Servicios>servicios;
    private View.OnClickListener listener;

    

    public adapter(List<Servicios> servicios) {
        this.servicios = servicios;

    }

    @NonNull
    @Override
    public Serviciosviewholder onCreateViewHolder(@NonNull ViewGroup parent, int i) {
        View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.itemview, parent,false);
        Serviciosviewholder holder = new Serviciosviewholder(v);

        v.setOnClickListener(this);

        return holder;
    }

    @Override
    public void onBindViewHolder(@NonNull Serviciosviewholder holder, int position) {

        Servicios servicios1= servicios.get(position);
        holder.txtNombre.setText(""+servicios1.getNombre());

        // holder.txtNombre.setText(servi.getNombre());



    }
    @Override
    public int getItemCount() {
        return servicios.size();//devulve el numero de filoas de reciclrevirewno
    }

    public void setOnclickListenes(View.OnClickListener listener){
        this.listener=listener;
    }
    @Override
    public void onClick(View v) {

       if (listener!=null){
      listener.onClick(v);

       }

    }

    public static class Serviciosviewholder extends RecyclerView.ViewHolder{

    TextView txtNombre;

    public Serviciosviewholder(@NonNull View itemView) {
        super(itemView);

      txtNombre=(TextView) itemView.findViewById(R.id.txt_Nom);


    }
}
}


este mi POJO

public class Servicios {

     private String Nombre;
     private float LatitudDestino; //float
     private float LongitudDestino;
     private float OrigenLat;
     private float OrigenLong;
     private String Status;
    Servicios() {}
    public Servicios(String nombre, float latitudDestino, float longitudDestino, float origenLat, float origenLong, String status) {
        Nombre = nombre;
        LatitudDestino = latitudDestino;
        LongitudDestino = longitudDestino;
        OrigenLat = origenLat;
        OrigenLong = origenLong;
        Status = status;
    }

    public String getNombre() {
        return Nombre;
    }

    public float getLatitudDestino() {
        return LatitudDestino;
    }

    public float getLongitudDestino() {
        return LongitudDestino;
    }

    public float getOrigenLat() {
        return OrigenLat;
    }

    public float getOrigenLong() {
        return OrigenLong;
    }

    public String getStatus() {
        return Status;
    }
}

