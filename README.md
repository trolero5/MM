package mercadillo.arranque;

import java.util.List;

import mercadillo.cuentas.ExcepcionCuenta;
import mercadillo.cuentas.sistemasfinancierosexternos.TipoSistemaFinancieroExterno;
import mercadillo.productos.CategoriaProducto;
import mercadillo.productos.EstadoProducto;
import mercadillo.usuarios.ControladorUsuarios;
import mercadillo.usuarios.ExcepcionUsuario;
import mercadillo.usuarios.MetodoMensajeria;
import mercadillo.productos.ControladorProductos;

/**
 * Clase con un main() de pruebas para la iteración 0, entregada por el profesor.
 *
 */
public class PruebasMercadillo1 {

	/**
	 * Método main(). No se esperan parámetros.
	 * @param args parámetros en línea de comandos, pero se ignoran.
	 * @throws ExcepcionCuenta 
	 */
	public static void main(String[] args) throws ExcepcionCuenta {
		//Crea una instancia de controlador de usuarios
		ControladorUsuarios cu = new ControladorUsuarios();
		//Crea una instancia de productos
		ControladorProductos cp = new ControladorProductos(cu);

		//Variables auxiliares para los identificadores de usuario
		String idBart = "bart";
		String idLisa = "lisa";
		String idBurns = "burns";

		////////////////////////////////////////////////////////
		// CASOS DE USO PREVIOS
		////////////////////////////////////////////////////////	
		try {
			// Caso de uso "crear socio"
			cu.crearSocio(idBart, "mola", "Bart", "Simpson", "742 Evergreen Terrace, Springfield", "00000000A", "666666666", "bart@tel.uva.es", MetodoMensajeria.SMS);
			cu.asociarCuentaExternaASocio(idBart, TipoSistemaFinancieroExterno.CUENTA_BANCARIA, "ES00 1000 1234 5678 9012 3456");
			cu.asociarCuentaExternaASocio(idBart, TipoSistemaFinancieroExterno.PAYPAL, "bartmoney");
			cu.crearSocio(idLisa, "bleedinggums", "Lisa", "Simpson", "742 Evergreen Terrace, Springfield", "22222222A", "666666667", "lisa@tel.uva.es", MetodoMensajeria.SMS);
			cu.asociarCuentaExternaASocio(idLisa, TipoSistemaFinancieroExterno.CUENTA_BANCARIA, "ES00 3000 3000 3000 3000 3000");
			cu.asociarCuentaExternaASocio(idLisa, TipoSistemaFinancieroExterno.PAYPAL, "lisamoney");
			cu.crearSocio(idBurns, "$$$", "Monty", "Burns", "Mansión Burns, Springfield", "11111111B", "000000000", "burns@tel.uva.es", MetodoMensajeria.CORREO);
			cu.asociarCuentaExternaASocio(idBurns, TipoSistemaFinancieroExterno.CUENTA_BANCARIA, "ES00 2000 3333 4444 5555 6666");
			cu.asociarCuentaExternaASocio(idBurns, TipoSistemaFinancieroExterno.PAYPAL, "burnsmoney");

			// Caso de uso "ingresar dinero CUENTA"
			cu.realizarIngresoEnCuentaSocio(idBart, (float)50.00, TipoSistemaFinancieroExterno.PAYPAL);
			cu.realizarIngresoEnCuentaSocio(idLisa, (float)250.00, TipoSistemaFinancieroExterno.PAYPAL);
			cu.realizarIngresoEnCuentaSocio(idBurns, (float)800.00, TipoSistemaFinancieroExterno.CUENTA_BANCARIA);
		} catch (ExcepcionUsuario eu) {
			//Si se llega hasta aquí alguna operación con usuarios ha ido mal
			System.out.println("Ha fallado una operación sobre el usuario con identificador '" + eu.getLogin() + "', por la siguiente causa: " + eu.getCausa().toString());
		} catch (ExcepcionCuenta ec) {
			//Si se llega hasta aquí alguna operación con tiendavideojuegos.tarjetas ha ido mal
			System.out.println("Ha fallado una operación sobre al realizar un movimiento con concepto '" + ec.getConcepto() + "', por la siguiente causa: " + ec.getCausa().toString());
		}

		////////////////////////////////////////////////////////
		// CASOS DE USO EN ESCENARIOS DE ÉXITO
		////////////////////////////////////////////////////////	
		System.out.println("\n===============================================");
		System.out.println("PRUEBAS DE LA ITERACIÓN 1 - ESCENARIOS DE ÉXITO");
		System.out.println("===============================================");

		//Variables auxiliares para tener listados y fichas
		List<String> listado;
		String ficha;
		/** NOTA DE LAS SIGUIENTES VARIABLES AUXILIARES: 
		 * se inicializan dentro del primer try/catch, pero el compilador considera que esa inicialización puede fallar y por eso
		 * si no se dan valores iniciales aquí dice que fuera del try/catch las variables no están inicializadas
		 */
		//Variables auxiliares para identificadores de descripciones
		String idDescCamisetaSimpson = "d1";
		String idDescCamisetaLisa = "d2";
		String idDescDisfraz = "d3";
		//Variables auxiliares para identificadores de ejemplares
		String idEjemCamiseta1 = "d1e1";
		String idEjemCamiseta2 = "d1e2";
		String idEjemCamiseta3 = "d1e3";
		String idEjemDisfraz1 = "d3e1";
		String idEjemDisfraz2 = "d3e2";
		//Variable auxiliar para identificador de lote
		String idLoteRopa = "l1";


		try {
			// Caso de uso "crear descripción de producto"
			System.out.println("\nCreo tres descripciones de producto, que recibirán los identificadores 'd1', 'd2' y 'd3'");
			System.out.println("----------------------------------------------");
			idDescCamisetaSimpson = cp.crearDescripcionProducto(idBart, "camiseta simpson", "Camisetas en las que aparecen dibujos de la serie 'Los Simpson'", CategoriaProducto.ROPA);
			System.out.println("Descripción creada con identificador: " + idDescCamisetaSimpson);
			idDescCamisetaLisa = cp.crearDescripcionProducto(idLisa, "camiseta lisa", "Camisetas sin dibujos", CategoriaProducto.ROPA);
			System.out.println("Descripción creada con identificador: " + idDescCamisetaLisa);
			idDescDisfraz = cp.crearDescripcionProducto(idBurns, "disfraz", "Disfraz de cualquier cosa", CategoriaProducto.DISFRACES);
			System.out.println("Descripción creada con identificador: " + idDescDisfraz);

			// Función de listar, que forma parte del caso de uso "ver descripción de producto"
			System.out.println("\nListo las descripciones");
			System.out.println("----------------------------------------------");
			listado = cp.listarDescripcionesProducto();
			for(String s : listado) {
				System.out.println(s);
			}

			//Caso de uso "ver descripción de producto"
			System.out.println("\nMuestro los datos completos de la descripción con identificador 'idDescDisfraz'");
			System.out.println("----------------------------------------------");
			ficha = cp.verDescripcionProducto(idDescDisfraz);
			System.out.println(ficha);

			//Caso de uso "crear ejemplar de producto"
			System.out.println("\nSe crean tres ejemplares de productos sobre la descripción con identificador 'idDescCamisetaSimpson'");
			System.out.println("----------------------------------------------");
			idEjemCamiseta1 = cp.crearEjemplarProducto(idBart, idDescCamisetaSimpson, (float)3.0, "camiseta de niño talla S con la cara de Bart", EstadoProducto.ROTO);
			System.out.println("Ejemplar creado con identificador: " + idEjemCamiseta1);
			idEjemCamiseta2 = cp.crearEjemplarProducto(idLisa, idDescCamisetaSimpson, (float)5.0, "camiseta de niña talla M con la cara de Lisa", EstadoProducto.CASI_NUEVO);
			System.out.println("Ejemplar creado con identificador: " + idEjemCamiseta2);
			idEjemCamiseta3 = cp.crearEjemplarProducto(idBurns, idDescCamisetaSimpson, (float)100.0, "camiseta de niño radiactiva con la cara del señor Burns", EstadoProducto.NO_FUNCIONA);
			System.out.println("Ejemplar creado con identificador: " + idEjemCamiseta3);
			System.out.println("\nSe crean dos ejemplares de productos sobre la descripción 'idDescDisfrazVampiro'");
			System.out.println("----------------------------------------------");
			idEjemDisfraz1 = cp.crearEjemplarProducto(idBart, idDescDisfraz, (float)20.0, "disfraz de Huckleberry Finn", EstadoProducto.USADO);
			System.out.println("Ejemplar creado con identificador: " + idEjemDisfraz1);
			idEjemDisfraz2 = cp.crearEjemplarProducto(idBurns, idDescDisfraz, (float)10.0, "disfraz del Conde Drácula", EstadoProducto.A_ESTRENAR);
			System.out.println("Ejemplar creado con identificador: " + idEjemDisfraz2);

			// Función de listar, que forma parte del caso de uso "ver ejemplar de producto"
			System.out.println("\nListo todos los ejemplares de la descripción 'idDescCamisetaSimpson'");
			System.out.println("----------------------------------------------");
			listado = cp.listarEjemplaresProducto(idDescCamisetaSimpson);
			for(String s : listado) {
				System.out.println(s);
			}
			System.out.println("\nListo todos los ejemplares de la descripción 'idDescCamisetaLisa'");
			System.out.println("----------------------------------------------");
			listado = cp.listarEjemplaresProducto(idDescCamisetaLisa);
			for(String s : listado) {
				System.out.println(s);
			}
			System.out.println("\nListo todos los ejemplares de la descripción 'idDescDisfrazVampiro'");
			System.out.println("----------------------------------------------");
			listado = cp.listarEjemplaresProducto(idDescDisfraz);
			for(String s : listado) {
				System.out.println(s);
			}

			//Caso de uso "ver ejemplar de producto"
			System.out.println("\nMuestro los detalles del ejemplar 'idEjemCamiseta1'");
			System.out.println("----------------------------------------------");
			ficha = cp.verEjemplarProducto(idEjemCamiseta1);
			System.out.println(ficha);

			//Caso de uso "crear lote"
			System.out.println("\nEl usuario 'bart' va a crear un lote con los productos 'idEjemCamiseta1' y 'idEjemDisfraz1' (que ha encontrado usando las operaciones de listado anteriores), ofreciendo un descuento del 20%");
			System.out.println("----------------------------------------------");
			idLoteRopa = cp.crearLoteProductos(idBart, "ropa de talla S", "contiene una camiseta y un disfraz de niño", (float)20.0);
			cp.adjuntarEjemplarALoteProductos(idLoteRopa, idEjemCamiseta1);
			cp.adjuntarEjemplarALoteProductos(idLoteRopa, idEjemDisfraz1);
			System.out.println("Lote creado con identificador: " + idLoteRopa);

			//Caso de uso "comprar ejemplar de producto"
			System.out.println("\nCompro un producto que en estos momentos no está en un lote");
			System.out.println("----------------------------------------------");
			cp.comprarEjemplarProducto(idBurns, idEjemCamiseta2);
			System.out.println("Ejemplar " + idEjemCamiseta2 + " comprado");

			// Caso de uso "listar movimientos de cuenta de socio" para comprobación
			System.out.println("\nListo los movimientos de la tarjeta del socio con identificador 'burns' para comprobar que la compra se ha hecho bien");
			System.out.println("----------------------------------------------");
			listado = cu.listarMovimientoCuentaSocio(idBurns);
			for(String s : listado) {
				System.out.println(s);
			}

			// Caso de uso "listar movimientos de cuenta de socio" para comprobación
			System.out.println("\nListo los movimientos de la tarjeta del socio con identificador 'lisa' para comprobar que la compra se ha hecho bien");
			System.out.println("----------------------------------------------");
			listado = cu.listarMovimientoCuentaSocio(idLisa);
			for(String s : listado) {
				System.out.println(s);
			}

			//Caso de uso "ver descripción de producto" para comprobación
			System.out.println("\nMuestro los datos completos de la descripción con identificador 'idDescCamisetaSimpson' para comprobar que hay un ejemplar menos");
			System.out.println("----------------------------------------------");
			ficha = cp.verDescripcionProducto(idDescCamisetaSimpson);
			System.out.println(ficha);

			//Caso de uso "ver ejemplar de producto" para comprobación
			System.out.println("\nMuestro los detalles del ejemplar 'idEjemCamiseta2' para comprobar que está vendido");
			System.out.println("----------------------------------------------");
			ficha = cp.verEjemplarProducto(idEjemCamiseta2);
			System.out.println(ficha);

		}catch (ExcepcionUsuario eu) {
			//Si se llega hasta aquí alguna operación con usuarios ha ido mal
			System.out.println("Ha fallado una operación sobre el usuario con identificador '" + eu.getLogin() + "', por la siguiente causa: " + eu.getCausa().toString());
		}/* catch (ExcepcionProducto ep) {
			//Si se llega hasta aquí alguna operación con productos ha ido mal
			System.out.println("Ha fallado una operación sobre la descripción de producto con identificador '" + ep.getIdentificador() + "', por la siguiente causa: " + ep.getCausa().toString() + (ep.getIdEjemplar()!=null ? (" (ejemplar " + ep.getIdEjemplar() + ")") : ""));
		} catch (ExcepcionCompra ec) {
			//Si se llega hasta aquí alguna operación con compras ha ido mal
			System.out.println("Ha fallado una operación sobre al intentar completar la compra de '" + ec.getConcepto() + "', por la siguiente causa: " + ec.getCausa().toString());
		}

		////////////////////////////////////////////////////////
		// CASOS DE USO EN ESCENARIOS DE FALLO
		////////////////////////////////////////////////////////	
		System.out.println("\n===============================================");
		System.out.println("PRUEBAS DE LA ITERACIÓN 1 - ESCENARIOS DE FALLO");
		System.out.println("===============================================");

		try {
			// Caso de uso "ver descripción de producto": se intenta mostrar una descripción con un identificador que no existe
			System.out.println("\nSe intenta mostrar una descripción que no existe");
			System.out.println("----------------------------------------------");
			ficha = cp.verDescripcionProducto("d999");
			System.out.println(ficha);
		} catch (ExcepcionProducto ep) {
			//Si se llega hasta aquí alguna operación con productos ha ido mal
			System.out.println("Ha fallado una operación sobre la descripción de producto con identificador '" + ep.getIdentificador() + "', por la siguiente causa: " + ep.getCausa().toString() + (ep.getIdEjemplar()!=null ? (" (ejemplar " + ep.getIdEjemplar() + ")") : ""));
		}

		try {
			// Caso de uso "crear ejemplar de producto": se intenta crear un ejemplar sobre una descripción que no existe
			System.out.println("\nSe intenta crear un ejemplar de una descripción que no existe");
			System.out.println("----------------------------------------------");
			cp.crearEjemplarProducto(idBart, "d999", (float)2.0, "da igual", EstadoProducto.A_ESTRENAR);
		} catch (ExcepcionUsuario eu) {
			//Si se llega hasta aquí alguna operación con usuarios ha ido mal
			System.out.println("Ha fallado una operación sobre el usuario con identificador '" + eu.getLogin() + "', por la siguiente causa: " + eu.getCausa().toString());
		} catch (ExcepcionProducto ep) {
			//Si se llega hasta aquí alguna operación con productos ha ido mal
			System.out.println("Ha fallado una operación sobre la descripción de producto con identificador '" + ep.getIdentificador() + "', por la siguiente causa: " + ep.getCausa().toString() + (ep.getIdEjemplar()!=null ? (" (ejemplar " + ep.getIdEjemplar() + ")") : ""));
		}

		try {
			// Caso de uso "ver ejemplar de producto": se intenta mostrar un ejemplar pero la descripción no existe
			System.out.println("\nSe intenta mostrar un ejemplar de una descripción que no existe");
			System.out.println("----------------------------------------------");
			ficha = cp.verEjemplarProducto("d999e0");
			System.out.println(ficha);
		} catch (ExcepcionProducto ep) {
			//Si se llega hasta aquí alguna operación con productos ha ido mal
			System.out.println("Ha fallado una operación sobre la descripción de producto con identificador '" + ep.getIdentificador() + "', por la siguiente causa: " + ep.getCausa().toString() + (ep.getIdEjemplar()!=null ? (" (ejemplar " + ep.getIdEjemplar() + ")") : ""));
		}

		try {
			// Caso de uso "ver ejemplar de producto": se intenta mostrar un ejemplar sobre una descripción que sí existe, pero no existe el ejemplar
			System.out.println("\nSe intenta mostrar un ejemplar inexistente de una descripción que sí existe");
			System.out.println("----------------------------------------------");
			ficha = cp.verEjemplarProducto(idDescDisfraz + "e9");
			System.out.println(ficha);
		} catch (ExcepcionProducto ep) {
			//Si se llega hasta aquí alguna operación con productos ha ido mal
			System.out.println("Ha fallado una operación sobre la descripción de producto con identificador '" + ep.getIdentificador() + "', por la siguiente causa: " + ep.getCausa().toString() + (ep.getIdEjemplar()!=null ? (" (ejemplar " + ep.getIdEjemplar() + ")") : ""));
		}

		try {		
			//Caso de uso "crear lote": se intenta adjuntar a un lote (ya existente) un producto que no existe
			System.out.println("\nSe intenta adjuntar a un lote un un ejemplar de producto que no existe");
			System.out.println("----------------------------------------------");
			cp.adjuntarEjemplarALoteProductos(idLoteRopa, idDescDisfraz + "e9");
		} catch (ExcepcionProducto ep) {
			//Si se llega hasta aquí alguna operación con productos ha ido mal
			System.out.println("Ha fallado una operación sobre la descripción de producto con identificador '" + ep.getIdentificador() + "', por la siguiente causa: " + ep.getCausa().toString() + (ep.getIdEjemplar()!=null ? (" (ejemplar " + ep.getIdEjemplar() + ")") : ""));
		}

		try {		
			//Caso de uso "crear lote": se intenta adjuntar a un lote (ya existente) un producto que no es del dueño del lote
			System.out.println("\nSe intenta adjuntar a un lote que pertenece a 'bart' un producto que pertenece a 'burns'");
			System.out.println("----------------------------------------------");
			cp.adjuntarEjemplarALoteProductos(idLoteRopa, idEjemDisfraz2);
		} catch (ExcepcionProducto ep) {
			//Si se llega hasta aquí alguna operación con productos ha ido mal
			System.out.println("Ha fallado una operación sobre la descripción de producto con identificador '" + ep.getIdentificador() + "', por la siguiente causa: " + ep.getCausa().toString() + (ep.getIdEjemplar()!=null ? ("(ejemplar " + ep.getIdEjemplar() + ")") : ""));
		}

		try {		
			//Caso de uso "comprar ejemplar de producto": se intenta comprar un producto que no existe
			System.out.println("\nSe intenta comprar un producto que no existe");
			System.out.println("----------------------------------------------");
			cp.comprarEjemplarProducto(idBurns, idDescDisfraz + "e9");
		} catch (ExcepcionUsuario eu) {
			//Si se llega hasta aquí alguna operación con usuarios ha ido mal
			System.out.println("Ha fallado una operación sobre el usuario con identificador '" + eu.getLogin() + "', por la siguiente causa: " + eu.getCausa().toString());
		} catch (ExcepcionProducto ep) {
			//Si se llega hasta aquí alguna operación con productos ha ido mal
			System.out.println("Ha fallado una operación sobre la descripción de producto con identificador '" + ep.getIdentificador() + "', por la siguiente causa: " + ep.getCausa().toString() + (ep.getIdEjemplar()!=null ? (" (ejemplar " + ep.getIdEjemplar() + ")") : ""));
		} catch (ExcepcionCompra ec) {
			//Si se llega hasta aquí alguna operación con compras ha ido mal
			System.out.println("Ha fallado una operación sobre al intentar completar la compra de '" + ec.getConcepto() + "', por la siguiente causa: " + ec.getCausa().toString());
		}

		try {		
			//Caso de uso "comprar ejemplar de producto": se intenta comprar un producto que ya está vendido
			System.out.println("\nSe intenta comprar un producto que ya está vendido");
			System.out.println("----------------------------------------------");
			cp.comprarEjemplarProducto(idBart, idEjemCamiseta2);
		} catch (ExcepcionUsuario eu) {
			//Si se llega hasta aquí alguna operación con usuarios ha ido mal
			System.out.println("Ha fallado una operación sobre el usuario con identificador '" + eu.getLogin() + "', por la siguiente causa: " + eu.getCausa().toString());
		} catch (ExcepcionProducto ep) {
			//Si se llega hasta aquí alguna operación con productos ha ido mal
			System.out.println("Ha fallado una operación sobre la descripción de producto con identificador '" + ep.getIdentificador() + "', por la siguiente causa: " + ep.getCausa().toString() + (ep.getIdEjemplar()!=null ? (" (ejemplar " + ep.getIdEjemplar() + ")") : ""));
		} catch (ExcepcionCompra ec) {
			//Si se llega hasta aquí alguna operación con compras ha ido mal
			System.out.println("Ha fallado una operación sobre al intentar completar la compra de '" + ec.getConcepto() + "', por la siguiente causa: " + ec.getCausa().toString());
		}

		try {		
			//Caso de uso "comprar ejemplar de producto": se intenta comprar un producto para el que no se tiene saldo suficiente
			System.out.println("\nSe intenta comprar un producto para el que no se tiene saldo suficiente");
			System.out.println("----------------------------------------------");
			cp.comprarEjemplarProducto(idBart, idEjemCamiseta3);
		} catch (ExcepcionUsuario eu) {
			//Si se llega hasta aquí alguna operación con usuarios ha ido mal
			System.out.println("Ha fallado una operación sobre el usuario con identificador '" + eu.getLogin() + "', por la siguiente causa: " + eu.getCausa().toString());
		} catch (ExcepcionProducto ep) {
			//Si se llega hasta aquí alguna operación con productos ha ido mal
			System.out.println("Ha fallado una operación sobre la descripción de producto con identificador '" + ep.getIdentificador() + "', por la siguiente causa: " + ep.getCausa().toString() + (ep.getIdEjemplar()!=null ? (" (ejemplar " + ep.getIdEjemplar() + ")") : ""));
		} catch (ExcepcionCompra ec) {
			//Si se llega hasta aquí alguna operación con compras ha ido mal
			System.out.println("Ha fallado una operación sobre al intentar completar la compra de '" + ec.getConcepto() + "', por la siguiente causa: " + ec.getCausa().toString());
		}*/
	}
}
