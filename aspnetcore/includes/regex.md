> [!WARNING]
> <span data-ttu-id="b6e79-101">Übergeben Sie ein Timeout, wenn Sie <xref:System.Text.RegularExpressions> zum Verarbeiten nicht vertrauenswürdiger Eingaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="b6e79-101">When using <xref:System.Text.RegularExpressions> to process untrusted input, pass a timeout.</span></span> <span data-ttu-id="b6e79-102">Ein böswilliger Benutzer kann Eingaben für `RegularExpressions` angeben, um einen [Denial-of-Service-Angriff](https://www.us-cert.gov/ncas/tips/ST04-015) durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="b6e79-102">A malicious user can provide input to `RegularExpressions` causing a [Denial-of-Service attack](https://www.us-cert.gov/ncas/tips/ST04-015).</span></span> <span data-ttu-id="b6e79-103">ASP.NET Core-Framework-APIs, die `RegularExpressions` verwenden, übergeben ein Timeout.</span><span class="sxs-lookup"><span data-stu-id="b6e79-103">ASP.NET Core framework APIs that use `RegularExpressions` pass a timeout.</span></span>