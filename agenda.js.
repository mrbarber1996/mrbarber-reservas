
const URL = 'https://jkcfcfkfrhbstdsogfcf.supabase.co';

const KEY = 'sb_publishable_62E1s1mOWelEuP4LMGjhjw_PTF7nrYX';

async function consultar(ruta, opciones = {}) {
  const respuesta = await fetch(
    URL + '/rest/v1/' + ruta,
    {
      ...opciones,
      headers: {
        apikey: KEY,
        'Content-Type': 'application/json',
        ...opciones.headers
      }
    }
  );

  const datos = await respuesta.json();

  if (!respuesta.ok) {
    throw new Error(datos.message || 'Error de conexión');
  }

  return datos;
}

export async function cargarServicios() {
  return consultar(
    'services?select=*&active=eq.true'
  );
}

export async function cargarHoras(
  fecha,
  servicio,
  barbero = null
) {
  const horas = await consultar(
    'rpc/available_slots',
    {
      method: 'POST',
      body: JSON.stringify({
        p_date: fecha,
        p_service_id: servicio,
        p_barber_id: barbero
      })
    }
  );

  return horas.map(hora => ({
    ...hora,
    hora_local: new Date(
      hora.slot_start
    ).toLocaleTimeString('es-CL', {
      timeZone: 'America/Santiago',
      hour: '2-digit',
      minute: '2-digit',
      hour12: false
    })
  }));
}
