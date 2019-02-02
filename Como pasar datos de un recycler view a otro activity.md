
import android.content.Context;
import android.content.Intent;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.support.v7.widget.DefaultItemAnimator;
import android.support.v7.widget.DividerItemDecoration;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Adapter;
import android.widget.AdapterView;
import android.widget.TextView;
import android.widget.Toast;

import com.example.gaspar.bgodriver.objetos.Servicios;
import com.example.gaspar.bgodriver.objetos.adapter;
import com.firebase.ui.database.FirebaseRecyclerAdapter;
import com.google.firebase.database.ChildEventListener;
import com.google.firebase.database.DataSnapshot;
import com.google.firebase.database.DatabaseError;
import com.google.firebase.database.DatabaseReference;
import com.google.firebase.database.FirebaseDatabase;
import com.google.firebase.database.Query;
import com.google.firebase.database.ValueEventListener;

import java.util.ArrayList;
import java.util.List;

import static com.example.gaspar.bgodriver.R.id.parent;
import static com.example.gaspar.bgodriver.R.id.recicler;
import static com.example.gaspar.bgodriver.R.id.start;

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


       context = itemView.getContext()

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

