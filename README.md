using System;
using System.Collections.Generic;
using System.Web.UI;

namespace testeAspNet
{
    public partial class _Default : Page
    {

        [Serializable]
        public class PedidoItemTemp
        {
            public int Item { get; set; }
            public int ProdutoId { get; set; }
            public string Nome { get; set; }
            public int Quantidade { get; set; }
            public decimal Preco { get; set; }
        }

        private List<PedidoItemTemp> PedidoTemp
        {
            get
            {
                if (Session["PedidoTemp"] == null)
                    Session["PedidoTemp"] = new List<PedidoItemTemp>();

                return (List<PedidoItemTemp>)Session["PedidoTemp"];
            }
            set
            {
                Session["PedidoTemp"] = value;
            }
        }
        protected void BindGrid()
        {
            int contador = 1;
            foreach (var item in PedidoTemp)
            {
                item.Item = contador++;
            }
            grd.DataSource = PedidoTemp;
            grd.DataBind();
        }

        protected void Page_Load(object sender, EventArgs e)
        {
            lbl.Text = Session.SessionID;
        }

        protected void Button1_Click(object sender, EventArgs e)
        {
            var pedido = PedidoTemp;

            pedido.Add(new PedidoItemTemp
            {
                ProdutoId = pedido.Count+1,
                Nome = $"Produto {pedido.Count+1}",
                Quantidade = 1,
                Preco = 25.00m
            });

            PedidoTemp = pedido;
            
            BindGrid();
        }
    }
}
